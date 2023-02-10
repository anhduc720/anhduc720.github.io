---
title: Third Party Cookies Trouble With Chrome And AWS ALB
date: 2022-06-28 10:15:53
slug: third-party-cookies-trouble
tags: ["aws", "alb", "cookies", "iframe"]
status: active
desc: "I have worked with a system that allows the client to create contact forms and embed them on a customer's site."
---

## The Bug
I have worked with a system that allows the client to create contact forms and embed them on a customer's site.

One day, I received a bug report. Client said that customer site can not display their form because the X-FRAME-OPTIONS error. And the story begins here

I think this is a individual error and the reason is that the client page does not allow our iframes to be displayed, So I said them to tell the customer to set the CSP header without having any more investigation.

But, the thing is not simple, our client reported again that the site still can not be displayed.
And I must start to investigate

## First step is reproduce the bug

I have test with firefox, no error, test with safari, no error. And then is chrome, bravo. the error is here. The from still display but when end-user click on submit button. This frame display error "refuse connect to xxxxx". And the devtool display X-FRAME-OPTIONS error. 

## More Detail
So, I know that is not problem with frame options header or csp header. Of course customer's site refuse to display our form due to X-FRAME-OPTIONS in our site. But its just because the post request to our system return 500 error. And the error page do not have X-FRAME-OPTIONS to display in a iframe.

Then I read the log about this 500 Request. And I realized that the web server cannot verify the CSRFT token.
The request have CSRF Token here, why it can not be verified ?

The cookies, yes, when looking the request again, I see no session cookies is send with the request.

And with few searching, I know that because chrome change it's behavior, cookies without SameSite properties will be dafault to SameSite:Lax, which prevent chrome to using this cookies when the form is inside an iframe.


## Something else
But, Chrome have changed this two years ago. Why we only have bug from now, Not one week or one year ago ?

Go deeper in system source code. I see that there is already have a fix in three years ago. This fix will skip CSRFT token verify when have no any cookies attached in the request.
And reason of that fix is because of safari on OSX and iOS. Safari default block all third party cookies. So the client and the previous Dev of this system decided to skip CSRFT verify in case no have any cookies. This is a quick fix and leave a security hole in the system but people is OK with it.

And this fix makes form functionality work when Chrome changes its policy in 2020.
Well, some wrong makes right, I don't know

Unfortunately, as the number of users increases, I have decided to add more server and turn on stickiness session on ALB to keep user interact with the same server each time.
And now the bug happen. With stickiness session on, we have additional two cookies. These cookies have "SameSite=None; Secured". So, Chrome feel free to put it on POST request form. And make the previous fix invalid because the cookies is not blank now.

So. This is our Bug

## Fix
When have clearly info about this Bug. Everything is simple. with some setting and a custom middleware ( Our system was written by rails 5). The bug is gone.

## Lesson
Be careful with the old classical system. A light touch can knock them down. Everything must be test carefully.
