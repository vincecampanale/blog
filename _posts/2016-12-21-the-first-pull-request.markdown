---
layout: post
title:  "The First Pull Request"
date:   2016-12-21
---

A couple days ago, I made a [post]() about Habitica, ["an open-source habit building program that treats your life like a Role Playing Game."](http://habitica.wikia.com/wiki/Habitica_Wiki). Key word (words?): "open-source."

In the open source system, code based bugs are considered especially heinous. On GitHub, the dedicated developers who investigate these difficult errors are members of an elite squad known as the Open Source Community. This is one of their stories. (*DUN DUN*)

#### The Issue
The day was December 13, 2016. I had been wanting to contribute to an Open Source project for a while, but being new to software development, I doubted my ability to actually make a contribution. Plus, I had no idea where to even begin. Then, I listened to an episode of Adventures in Angular that happened to be about Habitica that talked about them being open source! Having used Habitica for a while, I decided to check out their Github repo. I went to the issues tab, and scrolled for a bit. "With 320 issues, there's gotta be one there for a newbie", I thought. After perusing for a while, I found the "Labels" option and saw "status: issue: help welcome now" and "type: entry level coding". Perfecto...
There was an issue in the "help welcome now" category that looked pretty straightforward, it was titled "space missing in Gem gift message." It did have the "entry level coding" label, but come on, how hard can it be to fix a missing space?
So, I volunteered. One of the people in charge said "Go for it" and changed the label to "status: issue: in progress."
Game on.

#### The Fork
Habitica thankfully has an article on their Wiki dedicated to getting a local version up and running. I forked the habitica repo, cloned it locally, ran npm install (after struggling to get my node and npm updated to the most recent stable versions, but that's another blog post itself), then started a MongoDB server. `npm start` and boom, localhost:3000 was a fully operational local version of Habitica's website.

#### The Hack
The error I was tasked with fixing was pretty simple. The gem gift message consisted of two strings: the intro ("Hello <name>, <other name> has sent you ") and the number of gems ("<number> gems"). Combining these two strings, you get the message "Hello <name>, <other name> has sent you <number> gems". BUT! When the translators translated these strings to their language, they did not include the space at the end of the first string because they are not developers, and spaces like that do not matter to anyone except developers. So, in any other language, the message would read "Hello <name>, <other name> has sent you<number> gems!".  Time to save the day.

I deleted the intro and made the gift message one big string: "Hello <name>, <other name> has sent you <number gems!" and then changed the actual string building code to the following:
```
let messageSentContent = t('privateMessageGiftGemsMessage', {
   receiverName: receiver.profile.name,
   senderName: userToSendMessage.profile.name,
   gemAmount,
 });
 messageSentContent =  `\`${messageSentContent}\` `;
```
{: .language-javascript}

Now, there were no trailing spaces for the translators to keep track of so the error was not only fixed but it was also future-proof!

#### Pull Request, Attempt 0
I pushed the changes to my fork, and made a pull request.
A few hours later, somebody commented! "Surely someone thanking me for my great work," I thought.
Alas, an observant contributor noticed that the intro message I had deleted was actually used for both gifting gems and gifting subscriptions. Deleting it altogether may fix the gem gift spacing error, but it breaks the subscription message.

#### The Hack, Revisited
In order to preserve the intro, I used `.trim()` to remove the white space on the ends of both the intro and the gift message. Then I added a space between the two in the final construction of the string.
This solution allowed us to keep the intro and the gift message separate.

#### Pull Request, Attempt 1
Thinking this solution was sufficient, I made another Pull Request. Unfortunately, this PR was also denied because it didn't pass the build tests (oops) and the entire commit history of my development branch was in this commit (also oops).
Additionally, a different PR which was currently being merged was going to break the code, so my issue was put on standby.

#### Pull Request, Attempt 2 - "The Merged One"
This [wonderful stackoverflow post](http://stackoverflow.com/questions/2276686/git-only-push-up-the-most-recent-commit-to-github) showed me how to make a new branch and cherrypick a commit so my PR would only contain the relevant commit. Additionally, it turned out that the subscription gift wasn't even in use anywhere in the code base, so I was safe to just stick to the original solution of consolidating intro and gift message into one string. I reverted the code back to my original solution, committed it, made a new branch to cherrypick that commit, and waited for the other PR to get settled. Once the other PR was merged, I was ready to submit my PR once again.

This one didn't pass the build tests either.

After troubleshooting for a while on my own, I commented on the issue to ask for some help. A friendly fellow contributor instructed me on how to diagnose the issues with the tests and directed me to the lines I needed to fix -- big ups to that guy. The fixes were all syntax based, like removing empty lines ("padding") around brackets, adding a trailing comma to the final element of an object (which seems wrong, but that's what the linter wanted so \*shrug\*), and using an ES6 feature which allows you to declare a property only once if it is identical to its value (i.e. `object{ prop }` instead of `object{ prop: prop}`). I also had to rewrite some of the tests in order to match the new expected value (with the space corrected).

With the code base ready for my commit, all types of tests passing, and a nice clean commit, I was golden. I closed my most recent PR and created a fresh one.

I checked back on Github the next day and saw my most recent PR was closed. I mumbled some choice words and set myself to start from scratch once again. I opened the pull request to see why it was closed, and saw a little purple merge symbol along with a message that it was merged!

And he lived happily ever after...
