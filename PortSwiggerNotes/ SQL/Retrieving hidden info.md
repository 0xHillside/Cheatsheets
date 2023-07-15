# Retrieving hidden data
imagine a website thats for shopping and we ask it to display all the products from different categories but in this case we want the gifts category specifically and this is the URL

```https://insecure-website.com/products?category=Gifts```

This causes the application to make a SQL query to retrieve details of relevant products from the database and it would look something like this

```SELECT * FROM products WHERE category = 'Gifts' AND released = 1```

Now lets break this command down one by one
-   the astericks indicates to select all the details | *
-   These details are to come from the PRODUCTS table | `FROM products`
-   Specifically the products with category column |  `WHERE category`
-   and that are Released, this could be a hint | `AND released = 1`

It would seem like released acts as a restriction hence this would be the key to finding hidden products, we can also assume that for unreleased products it would be probably `released = 0` to find the unreleased products, and considering that this website doesnt have any protection against SQL injections an attacker could construct an attack like this

`https://insecure-website.com/products?category=Gifts'--
The result from this would be a SQL query that looks something like this
	`SELECT * FROM products WHERE category`
	```'Gifts'--' AND released = 1```

So how did we get to this solution?

Lets break down this query
-   The goal is to block out the `released`  Tag, considering that we already have our first idea which is adding the comment. It should also be noted that -- comments everything AFTER IT not before it, lets try that and see how it holds up .
	`https://insecure-website.com/products?category=Gifts'--`
	`SELECT * FROM products WHERE category = 'Gifts--' AND released = 1`

-  To our surprise what it did is that it inherently continued the command as if the comment -- function wasnt there, now to fix this what we need to look at is the apostrophes ( `'` ). they are the key that enables us to abuse this query?.

-   Through examination we know that the normal input would look something like this
	`https://insecure-website.com/products?category=Gifts`
	 `SELECT * FROM products WHERE category = 'Gifts' AND released = 1`

-   as we can see there 2 apostrophes are automatically added to close off and specify that Gifts tag, so maybe we can abuse this by inputting our OWN apostrophe after the word Gift followed by the comment, what this would do is that it would disregard the automatic apostrophe that is automatically placed due to the comment we have and it should look something like this



-   The parts that are green are the parts that have been commented out, it can be seen that the apostrophre that we added goes before the one that was added automatically, and the comment we added basically tells it to fuck off with the released tag, this successfully lets us abuse the query and tell the released tag sayonara

- now that the attacker (us duh lol) have found out a way to abuse this we can use other SQL commands like the following
```SELECT * FROM products WHERE category = 'Gifts' OR 1=1--' AND released = 1```
The modified query will return all items where either the category is Gifts, or 1 is equal to 1. Since 1=1 is always true, the query will return all items. at hand
