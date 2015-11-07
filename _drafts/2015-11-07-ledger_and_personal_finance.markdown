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


# Using ledger

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

    (1) % LEDGER_FILE=/tmp/foo.dat l b Assets
                27100 Rs  Assets
                 7500 Rs    Business
                 2400 Rs    Cash
                17200 Rs    Savings
    --------------------
                27100 Rs


What were my expenses this month? (sorted)

    (1) % LEDGER_FILE=/tmp/foo.dat l r -M --period-sort total Expenses
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

I spent the most on charity and rent. How much do I owe?

    (1) % LEDGER_FILE=/tmp/foo.dat l b Liabilities
                -5595 Rs  Liabilities:CC

How much do I withdraw on average per week?

(1) % LEDGER_FILE=/tmp/foo.dat l r -W -A @Withdrawal and Assets:Cash
01-Nov-15 - 07-Nov-15                        Assets:Cash                                          5400 Rs              5400 Rs
08-Nov-15 - 14-Nov-15                        <Adjustment>                                        -3900 Rs                    0
                                             Assets:Cash                                          2400 Rs              3900 Rs
15-Nov-15 - 21-Nov-15                        <Adjustment>                                        -6967 Rs                    0
                                             Assets:Cash                                          8500 Rs              5433 Rs


























