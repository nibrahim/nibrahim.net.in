---
layout: post
title: "Ledger CLI"
date: "2015-11-07 12:10:09 +0530"
---

This is a post describing my experiences with the command line accounting tool [ledger](http://ledger-cli.org/). My general preference for text based tools that "fit" into the UNIX ecosystem and which can work well with Emacs led me to this. The fact that it's written by [someone I admire a lot](http://johnwiegley.com/) was an additional reason. The post that follows is a description of how I use it and what I've learnt over the past year. There will be gaps in my usage and outright mistakes due to my lack of understanding of accounting concepts and ledger features so be warned.

The tool is fairly simple. It requires that you maintain a list of your transactions in a [double entry](https://en.wikipedia.org/wiki/Double-entry_bookkeeping_system) system. Double entry book keeping means that all transactions should have a debit account (one which receives money) and a credit account (from which money is deducted). At any point in time, the accounts should balance to zero. This took some getting used to for me but not too much and I've started thinking of my accounts in this way now.

## Basics

A typical input file (which I'll call the ledger file from now) will be a list of entries that look like this


    2014/10/21  Acme groceries
        Expenses:Groceries     500 Rs
        Assets:Cash            -500 Rs

This records that on the 21 of October 2014, I spent 500 Rs cash to buy groceries. It moves 500 Rs from the `Assets:Cash` account to the `Expenses:Groceries` account. It's also possible to have more than two accounts as long as the amounts balance. For example


    2015/01/15 Acme restaurant
       Expenses:Eating out     1000 Rs
       Assets:Cash             -500 Rs
       Assets:Savings          -500 Rs

This says that I spent 1000 Rs on a meal at Acme Restaurant and paid 500 Rs in cash and the remaining 500 directly from my savings account (via. my debit card).

Accounts are hierarchical and the hierarchies are split using the `:`. So, you'd have things like `Expenses:Groceries`, `Expenses:Internet` which are two accounts all under the higher level `Expenses` account. I use `Assets` for accounts that hold my assets (like my bank accounts etc.), `Expenses` for expenses, `Income` to track where my money is coming in from, `Liabilities` to track money I owe and `Loans` to track money owed to me. I also have an `Equity` account to "withdraw" my initial assets from. More on this later. I use an Emacs mode that comes along with ledger to add entries to the file and use git to version control it. I have a few simple shell aliases that make certain queries available with a small number of keystrokes so that I can run things easily. More on this too, later.

Some notes on the non tech. aspects of the whole "system". While there are many good applications out there on all possible platforms, people usually fail to keep their accounts due to a lack of discipline. They start off in full gusto, like I've done before with Gnucash and other tools, and then they run out of steam, falter and, also like I did with Gnucash and other tools, give up. Many try again but finally just let it go for good. The problem is, in summary, lack of discipline. There are two ways, as I see it, to fix this problem. The first is to construct tools that work even if one doesn't have discipline. So there are apps that allow you, with a smart phone, to add an accounting entry to a database with just a few taps. Others which track SMSs from your bank to maintain a running balance of your financial situation. While not especially disciplined in my daily life, my approach is to try to develop it rather than adjust my lifestyle and tools to not need it. So, I make it a point to note down cash transactions during the day and then finally at the end of the day, as part of a daily ritual, update my ledger file with the expenses of the day. Once a week, as part of a weekly ritual, I cross check my balances against my actual accounts in my bank and wallet to make sure that everything is "in sync". This took some trouble to get ready but after a month or so, the whole thing has become automatic. I've resisted the temptation to try and "remember" things to add into the file since I usually end up forgetting. Everything is either written down or in the file.

The first entry in my ledger is for Jul 1 2014 and I'm still going strong. That's over a year of information and there are around 1800 transactions in there so it seems to be working.

I have a directory called `~/notes/accounts/` which has several ledger files. There's one called `accounts.dat` which is the most heavily used. I also keep a separate one called `lycaeum.dat` which tracks expenses and income for my mentoring institute. This is version controlled using git and pushed to multiple locations as backup. I have a shell function called `l` defined like so


    l () {
            ledger -w -f ${LEDGER_FILE} -V --date-format='%d-%b-%y' $*
    }


This inovkes ledger with a few preset options like `-w` which prints output in a wide (132 column) format, `-f` to specify which ledger file to use. The `LEDGER_FILE` variable is set to `~/notes/accounts/accounts.dat`. The `-V` displays market values for commodities (something which I'll describe later) and finally, the `--date-format` to print dates in a way that I can read them quickly. Getting how much I have in my savings accounts would then be done like so


    $ l balance Assets:Savings

The top like of the ledger file is `;-*-ledger-*-` which means that Emacs opens it up in ledger-mode. This has a few useful keybindings that I use. e.g. `C-c` `C-a` to quickly add an entry and `C-c` `C-q` to fix alignment. It also has a lot more but I'm not really that heavy a user.


## Using ledger

Let's take a little scenario and describe how things might work. It's artificial but still representative of how I use it.

I wake up in the morning and look at my TODO list. I have to pay electricity bills today. I pay them online using my credit card and then add an entry like so

    2015/11/07 Acme power company
        Expenses:Electricity    1000 Rs
        Liabilities:CC

One of the accounts in an entry can be left blank and ledger will automatically fill it in. In this case, the entry is as if we entered `-1000 Rs` for the `Liabilities:CC` account.

I get some work done and receive a text message which says that one of my service providers has charged my credit card for a VPS that I have. So,


    2015/11/07 Acme Cloud 
        Expenses:Hosting    1500 Rs
        Liabilities:CC


After a while, I have to run out to get some groceries and stuff. I first withdraw some cash from the bank and then spend some of it at a few stores.
I come back with a few bills. I decide to update the journal right away.


    2015/11/07 Withdrawal
        Assets:Cash     3000 Rs
        Assets:Savings

    2015/11/07 Acme Groceries
        Expenses:Groceries    1500 Rs
        Assets:Cash

    2015/11/07 Acme Fisheries
        Expenses:Groceries:Meat    500 Rs
        Assets:Cash


This says that I withdrew 3000 Rs. from the savings bank account and then spent 2000 from that on some groceries and some fish (which I usually track separately). Back at home, I continue with my day and have to go out again. I refuel my motorcycle and keep the amount noted down. I also stop for a quick bite to eat from a small bakery. The resulting ledger entries are like so

    2015/11/07 Acme fuel
        Expenses:Petrol:Bike    500 Rs
        Assets:Cash

    2015/11/07 Acme bakery
        Expenses:Food          50 Rs
        Assets:Cash


After entering this, just to see if things are in sync, I run


    l b Assets:Cash


Commands to ledger can be abbreviated so `b` means `balance`. It says

         1,250.00 Rs  Assets:Cash

I check my wallet and see that there's just 1,150 Rs in there. I spent 100 Rs somewhere during the day which I'm not sure of and I quickly make an entry to balance to discrepancy

    2015/11/07 Acme fuel
        Assets:Cash    =1150 Rs
        Expenses:Unknown

The `=` in the amount here sets the amount in that account to the specified number and will add or deduct from the `Expenses:Unknown` account to make the `Assets:Cash` correct. This over time, accumulates accounting errors and ideally, it should be 0.

I get a text message indicating a bank transfer. A student from [The Lycaeum](http://thelycaeum.in/) has transferred a part of the fees for a mentoring course that I'm conducting. Okay


    2015/11/07 Mentoring course Fees
        Assets:Business      5000 Rs
        Income:Lycaeum:Fees


I keep a separate bank account for business transactions so this amount goes there and it's credited from the `Income:Lycaeum:Fees` account. Now, I need to book a few tickets for an upcoming training which I'm going to do. This means


     2015/11/07 Acme Bus service
         Expenses:Travel      1000 Rs
         Liabilities:CC


To make things more realistic, let's add a section to the top of our file like so

    2015/11/01 Initial balance
        Assets:Business    50000 Rs
        Assets:Savings    25000 Rs
        Assets:Cash        5000 Rs
        Equity

These are the initial amounts taken out of a special `Equity` account. This is how initial balances are handled.

I will also add similar entries for a few other days in the month so that the file is more interesting. I've annotated it with comments (the lines that start with `;`) for clarity.
The final file looks like this

<script src="https://gist.github.com/nibrahim/d632fc364c7acc9978df.js"></script>

And now, let's run some queries on it.

How much do I have in my various accounts? My assets.

    $ l b Assets
                27100 Rs  Assets
                 7500 Rs    Business
                 2400 Rs    Cash
                17200 Rs    Savings
    --------------------
                27100 Rs

The ledger tool takes a query type as its first argument. There are several query types. The one we've used above is `balance` (abbreviated to `b`). You can then pass a query expression which will limit the entries that are displayed. In this case, I've asked for the balance query for the Assets accounts. It shows me a list and a total. That's nice. Now, What were my expenses this month? (sorted)

    $ l r -M --period-sort total Expenses
    01-Nov-15 - 30-Nov-15                        Expenses:Food                                          50 Rs                50 Rs
                                                 Expenses:Unknown                                      100 Rs               150 Rs
                                                 Expenses:Maintenance                                  200 Rs               350 Rs
                                                 Expenses:Medicine                                     250 Rs               600 Rs
                                                 Expenses:Petrol:Bike                                  500 Rs              1100 Rs
                                                 Expenses:Entertainment                                595 Rs              1695 Rs
                                                 Expenses:Groceries:Meat                               700 Rs              2395 Rs
                                                 Expenses:Fine                                         900 Rs              3295 Rs
                                                 Expenses:Fitness                                     1000 Rs              4295 Rs
                                                 Expenses:Travel                                      1000 Rs              5295 Rs
                                                 Expenses:Entertainment:Restaurant                    1500 Rs              6795 Rs
                                                 Expenses:Hosting                                     1500 Rs              8295 Rs
                                                 Expenses:Calligraphy                                 2500 Rs             10795 Rs
                                                 Expenses:Groceries                                   2700 Rs             13495 Rs
                                                 Expenses:Domestic                                    3500 Rs             16995 Rs
                                                 Expenses:Maintenance:Washing machine                 4000 Rs             20995 Rs
                                                 Expenses:Charity                                     5000 Rs             25995 Rs
                                                 Expenses:Rent                                        5000 Rs             30995 Rs

This time, we use the `register` (abbreviated to `r`) query. It's probably the most heavily used one. Here, we pass `-M` to group transactions by month. By default, there's no grouping and all transactions are displayed. Here, we want it to group so that we can further process it. We then provide the `--period-sort` option which will sort the entries in the group by the given parameter. We sort by "total" (which means total amount). After this, I use "Expenses" since that's what I'm interested in and find out that I spent the most on charity and rent.

How much do I owe?

    $ l b Liabilities
                -5595 Rs  Liabilities:CC

A simple balance query on Liabilities. How much do I withdraw on average per week?

    $ l reg -A  -W Assets:Cash and @Withdraw
    01-Nov-15 - 07-Nov-15                        Assets:Cash                                          5400 Rs              5400 Rs
    08-Nov-15 - 14-Nov-15                        <Adjustment>                                        -3900 Rs                    0
                                                 Assets:Cash                                          2400 Rs              3900 Rs
    15-Nov-15 - 21-Nov-15                        <Adjustment>                                        -6967 Rs                    0
                                                 Assets:Cash                                          8500 Rs              5433 Rs

This is another register query. The first column in a register query is the transaction amount. The second is usually a running total. The `-A` flag converts that to show a running average rather than a running total. The `-W` asks ledger to group things weekly (we used `-M` for montly earlier). The query expression is a little more complicated. It says "all transactions in touching the Assets:Cash account *and* which have Withdraw in the description". The `@` matches against descriptions rather than account names. The final 5344 Rs. tells us that we withdraw that much every week.


## Automated transactions.

Ledger can also automatically perform transactions based on criteria. I have an entry like so

    = /^Income/
        (Liabilities:Service tax)                 0.14


Which means, for all transactions that start with "Income", put 14% into a `Liabilities:Service tax` amount. This means that if I have a transaction like so


    2015/10/10
        Assets:Business   1000 Rs
        Income:Consulting

where the amount against `Income:Consulting` is -1000 Rs, ledger would automatically credit 140 Rs from the `Liabilities:Service tax` account. I can then quickly see how much I owe as service tax.

## Budgeting

This is a feature that I don't use enough yet but which I've started with. The basic idea is to create a budget for each account up front and then check against that. Let's try that with. A version of the accounts file with added budgets is shown below

<script src="https://gist.github.com/nibrahim/7a44c81066e69f7ab603.js"></script>

The added section is

    ~ Monthly
        Expenses:Rent                               5000 Rs
        Expenses:Groceries                          3500 Rs
        Expenses:Domestic                           4000 Rs
        Expenses:Electricity                        1500 Rs
        Expenses:Entertainment                      2000 Rs
        Expenses:Food                               1000 Rs
        Expenses:Fitness                             500 Rs
        Expenses:Hosting                            2500 Rs
        Expenses:Medicines                          1000 Rs
        Expenses:Petrol                             5000 Rs
        Assets


This tells ledger that every month, it should allocate this much money for each of the accounts specified and all of it comes from the `Assets` account. We could (and usually also do) have a `~ Yearly` budget.

A register query for the current month for expenses like so. The `-p` allows us to narrow the date range to the current month.

    $ l r -p "this month" Expenses
    01-Nov-15 Acme groceries                     Expenses:Groceries                                    500 Rs               500 Rs
    02-Nov-15 Acme Electricals                   Expenses:Maintenance                                  200 Rs               700 Rs
    02-Nov-15 Acme stationeries                  Expenses:Calligraphy                                 2500 Rs              3200 Rs
    04-Nov-15 Acme fine dining                   Expenses:Entertainment:Restaurant                    1500 Rs              4700 Rs
    06-Nov-15 Traffic fine                       Expenses:Fine                                         900 Rs              5600 Rs
    07-Nov-15 Acme Cloud                         Expenses:Hosting                                     1500 Rs              7100 Rs
    07-Nov-15 Acme Groceries                     Expenses:Groceries                                   1500 Rs              8600 Rs
    07-Nov-15 Acme Fisheries                     Expenses:Groceries:Meat                               500 Rs              9100 Rs
    07-Nov-15 Acme fuel                          Expenses:Petrol:Bike                                  500 Rs              9600 Rs
    07-Nov-15 Acme bakery                        Expenses:Food                                          50 Rs              9650 Rs
    07-Nov-15 Acme fuel                          Expenses:Unknown                                      100 Rs              9750 Rs
    07-Nov-15 Acme Bus service                   Expenses:Travel                                      1000 Rs             10750 Rs
    08-Nov-15 Acme Groceries                     Expenses:Groceries                                    700 Rs             11450 Rs
    08-Nov-15 Acme meat products                 Expenses:Groceries:Meat                               200 Rs             11650 Rs
    10-Nov-15 Acme save the whales fund          Expenses:Charity                                     5000 Rs             16650 Rs
    12-Nov-15 Gog                                Expenses:Entertainment                                595 Rs             17245 Rs
    15-Nov-15 Domestic help                      Expenses:Domestic                                    3500 Rs             20745 Rs
    17-Nov-15 Acme medicals                      Expenses:Medicine                                     250 Rs             20995 Rs
    18-Nov-15 Acme repair company                Expenses:Maintenance:Washing machine                 4000 Rs             24995 Rs
    21-Nov-15 Acme Housing                       Expenses:Rent                                        5000 Rs             29995 Rs
    25-Nov-15 Acme Gym                           Expenses:Fitness                                     1000 Rs             30995 Rs

Now, let's see how this balances against our budgets. This is as simple as adding the `--budget` flag to the query

    $ l r --budget Expenses
    01-Nov-15 Budget transaction                 Expenses:Rent:House                                 -5000 Rs             -5000 Rs
    01-Nov-15 Budget transaction                 Expenses:Groceries                                  -3500 Rs             -8500 Rs
    01-Nov-15 Budget transaction                 Expenses:Domestic                                   -4000 Rs            -12500 Rs
    01-Nov-15 Budget transaction                 Expenses:Electricity                                -1500 Rs            -14000 Rs
    01-Nov-15 Budget transaction                 Expenses:Entertainment                              -2000 Rs            -16000 Rs
    01-Nov-15 Budget transaction                 Expenses:Food                                       -1000 Rs            -17000 Rs
    01-Nov-15 Budget transaction                 Expenses:Fitness                                     -500 Rs            -17500 Rs
    01-Nov-15 Budget transaction                 Expenses:Hosting                                    -2500 Rs            -20000 Rs
    01-Nov-15 Budget transaction                 Expenses:Medicines                                  -1000 Rs            -21000 Rs
    01-Nov-15 Budget transaction                 Expenses:Petrol                                     -5000 Rs            -26000 Rs
    01-Nov-15 Acme groceries                     Expenses:Groceries                                    500 Rs            -25500 Rs
    04-Nov-15 Acme fine dining                   Expenses:Entertainment                               1500 Rs            -24000 Rs
    07-Nov-15 Acme Cloud                         Expenses:Hosting                                     1500 Rs            -22500 Rs
    07-Nov-15 Acme Groceries                     Expenses:Groceries                                   1500 Rs            -21000 Rs
    07-Nov-15 Acme Fisheries                     Expenses:Groceries                                    500 Rs            -20500 Rs
    07-Nov-15 Acme fuel                          Expenses:Petrol                                       500 Rs            -20000 Rs
    07-Nov-15 Acme bakery                        Expenses:Food                                          50 Rs            -19950 Rs
    08-Nov-15 Acme Groceries                     Expenses:Groceries                                    700 Rs            -19250 Rs
    08-Nov-15 Acme meat products                 Expenses:Groceries                                    200 Rs            -19050 Rs
    12-Nov-15 Gog                                Expenses:Entertainment                                595 Rs            -18455 Rs
    15-Nov-15 Domestic help                      Expenses:Domestic                                    3500 Rs            -14955 Rs
    25-Nov-15 Acme Gym                           Expenses:Fitness                                     1000 Rs            -13955 Rs

Now the numbers look different. What ledger does is that it deducts each budget item from the appropriate account thereby reducing it and then actual expenses increase it again. If the final total is negative, it means that you're below budget. If it's above, it means that you've crossed your budget. To clarify, let's look at just one account

    $ l r --budget Expenses:Rent
    01-Nov-15 Budget transaction                 Expenses:Rent                                       -5000 Rs             -5000 Rs
    21-Nov-15 Acme Housing                       Expenses:Rent                                        5000 Rs                    0

Tells us that we had 5000 Rs budgeted for the rent and it was fully spent. Not so for entertainment

    $ l r --budget Expenses:Entertainment
    01-Nov-15 Budget transaction                 Expenses:Entertainment                              -2000 Rs             -2000 Rs
    04-Nov-15 Acme fine dining                   Expenses:Entertainment                               1500 Rs              -500 Rs
    12-Nov-15 Gog                                Expenses:Entertainment                                595 Rs                95 Rs

where we overshot our budget by 95 Rs. However, for Domestic expenses, we are under budget

    $ l r --budget Expenses:Domestic
    01-Nov-15 Budget transaction                 Expenses:Domestic                                   -4000 Rs             -4000 Rs
    15-Nov-15 Domestic help                      Expenses:Domestic                                    3500 Rs              -500 Rs

Viewing everything like we first did gives us the information that we're within our budget which is good.


## Concluding remarks

Like I said above, I've been using the tool for a year now. The whole thing has worked for me and as of today, I have around 30k in my `Unknown` account. That's not very good but since this is the first time I'm keeping accounts and this is over a whole year, it's not that bad either.

It's an ideal tool for UNIX aficionados. It's text based, command line driven, super fast, has a somewhat steep learning curve with great rewards at the end, extremely flexible, transparent about what it does and a few other things which characterise the kind of software I like but that's for another post. The official site is [ledger-cli.org](http://ledger-cli.org/). The [manual](http://ledger-cli.org/3.0/doc/ledger3.pdf) is worth reading just for itself and is fairly complete.



