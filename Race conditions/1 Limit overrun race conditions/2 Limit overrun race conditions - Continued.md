
If you later attempt to reuse this code, the initial checks performed at the start of the process should prevent you from doing this:

![Discount code validation process](https://portswigger.net/web-security/race-conditions/images/race-conditions-discount-code-normal.png)

Now consider what would happen if a user who has never applied this discount code before tried to apply it twice at almost exactly the same time:

![Race condition in discount code validation](https://portswigger.net/web-security/race-conditions/images/race-conditions-discount-code-race.png)

As you can see, the application transitions through a temporary sub-state; that is, a state that it enters and then exits again before request processing is complete. In this case, the sub-state begins when the server starts processing the first request, and ends when it updates the database to indicate that you've already used this code. This introduces a small race window during which you can repeatedly claim the discount as many times as you like.