# sqaure-hack-2023

## Inspiration
Invoice Tracker V1 is useful, but it takes some efforts to key in all the invoices.  Using Square Invoice API helps to reduce the efforts.
-

## What it does
Retrieve invoices using Square API
Track all invoices in one place
Identify which invoices are past due and how late they are
Calculate how much amount is outstanding
Calculate expected payments in immediate future
Easy to identify due amounts by Customer
Simple and easy to use
-

## How we built it
We use Invoice API and Google Sheet Apps Script to build this new version of invoice tracker.  
-

## Challenges we ran into
We did not run into much challenges except one formula uses subtotal() function assuming the invoice number is of type NUMBER whereby the invoice number retrieved from API is of type TEXT.  Changing the function code of subtotal() function from 2 to 3 solves the problem.
-

## Accomplishments that we're proud of
We made the Invoice Tracker V2 working.
-

## What we learned
We learned the Invoice API, Apps Script and some Google Sheet's shortcuts.
-

## What's next for Square + Invoice Tracker
Add Square Estimate and convert estimates to invoices.
Send reminders from Invoice Tracker.
-
