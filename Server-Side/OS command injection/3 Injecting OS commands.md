
In this example, a shopping application lets the user view whether an item is in stock in a particular store. This information is accessed via a URL:

`https://insecure-website.com/stockStatus?productID=381&storeID=29`

To provide the stock information, the application must query various legacy systems. For historical reasons, the functionality is implemented by calling out to a shell command with the product and store IDs as arguments:

`stockreport.pl 381 29`

This command outputs the stock status for the specified item, which is returned to the user.