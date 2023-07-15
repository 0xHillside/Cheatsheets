# Subverting application logic allowing a login bypass

## Scenario 
There is a application in this case that lets users log in with a username and password. If the user submits the user name for example *weiner (lol penis joke)* and the password *blue cheese (woulda been funnier if it was poopfart)* then the SQL structure would look like the following
`SELECT * FROM users WHERE username = 'wiener' AND password = 'bluecheese'`

if the query returns the details of the user, then the login is successful, otherwise it would be rejected. Here an attacker can log in as any user without a password simply by using the SQL comment sequence `--` to remove the password check from the `WHERE` clause.

another way to understand it is to remember that everything after the comment gets treated as a comment hence everything right after the `username = administrator'--` would be treated as such basically disabling the password checking entirely as this is a different way to understand it 


the SQL command we know will look something like this;

``` SELECT firstname FROM users where username = 'admin' and password = 'admin'```

we know this as the default so to subvert the logic what we do simply is to change it so that it becomes this

```SELECT firstname FROM users where USERNAME = 'admin'--' and password = 'admin'```

what this does now is that when it runs the query everything after the comment gets removed, so everything after ``` '-- ``` is commented out and doesn't run.
