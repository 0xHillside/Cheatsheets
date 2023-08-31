
Afew words to be said is that this sql attack is very different from the rest, it is a series of process to reach the result, nothing too fancy just afew building blocks that are needed that are knowing how many columns there are, which allow the data type we want and how do we extract that information.

There are two possible ways to determine this.

### Null guessing submission
This method involves submitting a series of UNION SELECT, payloads with a specific number of NULLS. 

The code would look something like this
```
' UNION SELECT NULL--
' UNION SELECT NULL,NULL--
' UNION SELECT NULL,NULL,NULL--
```

You MUST realize the fact that the number of lines corresponds to the number of columns meaning if you get have 4 nulls and it gives you and error then you made a wrong guess, but if you have 3 nulls and there is no error then it means that there are 3 columns. lets try to apply this to the lab

We know from the description of the lab that our current target scope is the category filter, admittedly I didn't pay attention to that and had tested everything else except for it, but fuck it we ball lets move on

normally the URL would look something like `/filter?category=Pets` so lets add an apostrophe to test if there is a vulnerability like so  `/filter?category=Pets'`

![[Pasted image 20230802185455.png|600]]

If it isn't already obvious we have successfully tested to see if its vulnerable and with this error its confirmed it is, now to concoct our payload. We know that the payloads as we discussed are `' UNION SELECT NULL--` , all we have to do is now add this becoming  `/filter?category=Pets' UNION SELECT NULL--` and once we input this the result is

![[Pasted image 20230802190006.png|675]]

The reason why we received an error is because there isnt only ONE column here, which means we must add another `/filter?category=Pets' UNION SELECT NULL,NULL--`

![[Pasted image 20230802190159.png]]

Okay now we know there arent just two columns here, but rather more so we add another NULL to see what happens.
`/filter?category=Pets' UNION SELECT NULL,NULL,NULL--`

![[Pasted image 20230802190317.png|725]]

as we can see the fact that we don't receive an error means that there are specifically 3 columns here.