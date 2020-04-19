---
layout: post
title: Revamped Espial IDS
date: '2013-06-27T09:59:00.003-06:00'
author: JM
tags:
- Espial
- programming
- IDS
- intrusion detection
- development
- coding
- intrusion detection system
modified_time: '2013-06-27T13:13:33.170-06:00'
blogger_id: tag:blogger.com,1999:blog-4063990595241430327.post-4515264940048877201
blogger_orig_url: http://www.pwnage.io/2013/06/revamped-espial-ids.html
---

## Intro

I was taking another look at the [code I wrote for my university project a while back](http://infosec4breakfast.blogspot.ca/2012/08/espial-ids_12.html) and honestly I didn't like what I saw. After coding Ruby for a while, I see that simplicity is best in most circumstances, and having a huge amount of code pertaining to things like linked lists for rule lookups isn't really what this project should be about. I also hated the fact that rules had to be based on single key words - since at the time I thought efficiency had to be priority - but this should no in fact be the case. It's a IDS for crying out loud! So I decided to re-write the thing to use a number of different methods for rule lookups, and rule settings to include basically whatever you want.

## Overview

I just wanted to make this post to provide a general overview of what's going on. Basically what this does is it takes rules based on this format:

`^!!START_MATCH!!(.*)!!END_MATCH!!::([0-9])::([a-zA-Z!.? ]+)`

As you can probably guess, the signature is put in between the `START_MATCH` and `END_MATCH` areas. I've then implemented a packet multiplier for future functionality, and the alert itself. What I would like to see happen is having parsing for TCPDump to include the originating IP address, however, right now it just includes where the match occurred.

### Regex Signatures

I've also decided to make the signatures regex based. So if you're matching something like HTML, you'll have to escape all of the areas that would affect the regex itself.

### Fast Rule Lookups With Regex and Vectors

So, I still want this thing to be fast. Currently the match block is constructed like so:

```
>     if(this->rules->size() > 0) {
>         string searchBlock = "";
>         for(rulesIter = this->rules->begin(); rulesIter != this->rules->end(); rulesIter++) {
>             if(rulesIter == this->rules->begin()) //Construct a big fat regex from all of the rules.
>                 searchBlock += ".*(" + rulesIter->first + ").*";
>             else                searchBlock += "|.*(" + rulesIter->first + ").*";
>         }
>         this->globalMatchBlock = newregex(searchBlock);
```

Basically all this does is stick them all together into one OR statement regex to match text on. The result is that the match block will stop on the first match that occurs, and the resultant regex array will only contain text at index (if you want to call it that) where the match occurred. So for example:

`.*(match).*|.*(me).*|.*(please).*`

If I have the text of "match" then the array index [1] will have "match" in it. Which resulted in this function:

```
> voidDB::search(string &currLine)
> {
>     smatch result;
>     ofstream outfile;
>     regex_search(currLine, result, *globalMatchBlock);
>    
>     for(int i = 0; i < result.size(); i++){
>         if(result[i].length() > 2 && i != 0) {
>             outfile << "Match: " << (*rulesVec)[i-1].alert << endl; //Access alert in vector at that index, since that's the one that matched...
>             outfile << "From: " << result[0] << endl << endl;
>            
>             outfile.flush();
>        }
```

 So basically it will search through the text, and if the array index contains a match, it will look up the alert in the established rule vector, resulting in O(n)+O(1) lookup efficiency. The initial O(n) comparison seems slow from an algorithm perspective, but the length comparisons won't use up much CPU time compared to a rule comparison against a map, or hash map.

### Limitations

The limitations are that the string buffer is a limited size, since only one match will occur based on the provided buffer (currLine). Another limitation is that one rule is only being matched per block. So if you had multiple rules being matched in a provided text block then you would only see one match occurring.

## The Project and Getting Involved

You can find the project on my GitHub: [https://github.com/jershmagersh/EspialIDS/](https://github.com/jershmagersh/EspialIDS/tree/master/EspialIDS)

Currently there isn't much of a ReadMe, and other necessities, but these will come with time. If you'd like to get involved please contact me. Currently the only developer involved is myself.
