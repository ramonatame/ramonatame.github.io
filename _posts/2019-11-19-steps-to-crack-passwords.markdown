---
layout: post
title:  "Steps to Crack Passwords"
date:   2019-11-19
---

There are many articles out there explaining what password cracking means, tools that you can use and many other useful thoughts, but there aren’t many resources containing practical steps to crack passwords. I decided that whenever I am working on something less documented out there than I would like, I will publish my own scripts and articles. Hence, this article explains password cracking step by step, based on [my password cracking script](https://github.com/ramonatame/password-cracking-script/blob/master/passwords_cracking_script.sh).

Hopefully it’s obvious for anyone reading articles on this topic – password cracking is prosecuted if used illegally. This article is meant for the defensive teams and legal red teams.

*Let’s get started.*

## #1 Install Hashcat or another password cracking tool

Out of the many available tools, hashcat feels simple and yet powerful enough for password cracking. All the concepts in this article apply no matter the password cracking tool, though you will find here only the commands for hashcat.

## #2 Create your baseline files: dictionaries, rules, masks

For password cracking you need to start from lists containing dictionaries, rules and masks. You don’t have to start from scratch, you can use a bunch of open source tools such as Hashcat and JohnTheRipper for their default rules and dictionaries, Mentalist and Crunch for customized word lists, Cewl for crawling your website for company related keywords and Pipal for statistics given a list with cracked passwords. I use and appreciate all these tools, trust me, it worths looking into them.

### #2.1 Create your dictionaries

Using the tools mentioned above or others, you can create a compilation of any lists you find relevant on different topics people use within passwords, such as: worst leaked passwords, other leaked passwords, top 10k English words, your company related words, common basewords, colors, emotions, explicit, slang, violence, pets, family, first names, military, months, week days, seasons, sports, vehicles, keyboard combinations and anything else you can think of. I also prefer to sort them by length and frequency to help a bit the CPU/ GPU try the easiest ones first and only if needed the most complex ones.

### #2.2 Create the rules

I use a compilation of rules, grabbed from all over the places or generated with the above tools. They contain rules for appending/ prepending numbers and/ or symbols, case change, appending days/ years, substitutions and so on. Hashcat comes with a set of predefined rules as well, I just prefer to add a few more.

### #2.3 Create the masks

Create a set of masks or gather them using the above tools, depending on your password strength policy. Masks are patterns used for brute-force attacks. Instead of trying all the possible character combinations for a password of a predefined length, you would configure a set of patterns (i.e. try to match a password of 8 characters, having 6 letters, then a number, then a symbol). Preferably, you would find out the password policy for your company (rules for passwords) and you would try only that pattern or make sure it is part of your masks list.

## #3 Find out the password hashing algorithm

Before you start cracking the passwords, you need to know how did a plain text password become that unfortunate string. You need to find out the hashing algorithm and what salt/ pepper was used if any. Why? Because any common password cracking tool will require you to specify how where the passwords hashed, otherwise the computational work is imposible. The easiest way to find out these is just ask the people in your company who designed the authentication mechanism. If this is not an option, you can use tools like the hash-identifier.

Just in case you wonder, generally you would assume an attacker got not only your list with hashed passwords, but also the salt/ pepper lists together with them and he is able to find out the hashing algorithm one way or another.

## #4 Crack numerical passwords

Trying to match a small subset of characters such as numbers only is faster than trying to go through huge lists of predefined words, rules or some kind of brute-forcing. I start the password cracking by checking if any of the passwords are made of numbers only, up to a length my computer resources are comfortable with.

`
hashcat -m $hash_mode -a 3 -o $outfile $hashlist $pwd_utils_dir/rules/numbers_ruletheall.rules --remove --force
`

## #5 Run dictionary attack by frequency

Next I check if I can crack any passwords from my list using a simple dictionary attack. For dictionary attacks, you would have a large list of possible passwords and you would iterate through them to see if that is the password. A common source for dictionaries is leaked passwords, posted on open source websites such as hashes.org. You could grab some lists from there or other places and use them. I prefer to order the list based on length and frequency, so it will match earlier a weaker password and stop trying the more complex ones. See step 2.1. for more details.

`
hashcat -m $hash_mode -a 0 -o $outfile $hashlist $pwd_utils_dir/wordlists/worst/all_worst --force --remove
hashcat -m $hash_mode -a 0 -o $outfile $hashlist $pwd_utils_dir/wordlists/common/cat_all_common_words --force --remove
hashcat -m $hash_mode -a 0 -o $outfile $hashlist $pwd_utils_dir/wordlists/dict/top10k_english.txt --force --remove
hashcat -m $hash_mode -a 0 -o $outfile $hashlist $pwd_utils_dir/wordlists/leaked/final_lists/all_leaked_by_freq  --force --remove
`

## #6 Run hybrid attack with rules

A hybrid attack needs a list with possible password and a list with rules or patterns to apply on top of them. For example, if the dictionary list contains “password”, given the right rules, a hybrid attack would also try “Password”, “p4ssw0rd” and so on. Here you would use the lists you created at step 2.1. and step 2.2.

`
hashcat -m $hash_mode -a 0 -o $outfile $hashlist $pwd_utils_dir/wordlists/worst/all_worst -r $pwd_utils_dir/rules/cat_all_custom_rules.rule --force --remove
hashcat -m $hash_mode -a 0 -o $outfile $hashlist $pwd_utils_dir/wordlists/common/cat_all_common_words -r $pwd_utils_dir/rules/cat_all_custom_rules.rule --force --remove
hashcat -m $hash_mode -a 0 -o $outfile $hashlist $pwd_utils_dir/wordlists/dict/top10k_english.txt -r $pwd_utils_dir/rules/cat_all_custom_rules.rule --force --remove
hashcat -m $hash_mode -a 0 -o $outfile $hashlist $pwd_utils_dir/wordlists/leaked/final_lists/all_leaked_by_freq -r $pwd_utils_dir/rules/cat_all_custom_rules.rule --force --remove
`

## #7 Run brute-force for short passwords

Running brute-force for short passwords might be less computational expensive than running more complex attacks. Based on different forums and tests, I decided that 6 character length is the limit I can account as a short password. As such, before moving over to more complex steps, I prefer to run this one for short passwords.

`
hashcat -m $hash_mode -a 3 -o $outfile $hashlist $pwd_utils_dir/masks/all_up_to_6 --remove --force
`

## #8 Run a simple combined attack

The combined attack requires you to give it at least 2 word lists sources and it will iterate through each of them to get all the possible combinations and will check if any of these combinations represents the password you are trying to crack.

`
hashcat -m $hash_mode -a 1 -o $outfile $hashlist $pwd_utils_dir/wordlists/worst/all_worst $pwd_utils_dir/wordlists/common/cat_all_common_words --force --remove
hashcat -m $hash_mode -a 1 -o $outfile $hashlist $pwd_utils_dir/wordlists/common/cat_all_common_words $pwd_utils_dir/wordlists/worst/all_worst --force --remove
`

## #9 Run a combined attack with rules

Concatenating 2 words might crack some of the passwords, but certainly a good number of people creates passwords made of 2 words with some changes, such as adding a number or symbol at the beginning, in between or end of the password or any other hybrid rule mentioned at step 2.2. So the next thing I try is to run a combined attack as in step 8, but including the defined rules and iterate through all the possibilities added by working with rules. Hopefully you can see how the complexity of the password cracking increases.

`
hashcat -m $hash_mode -a 1 -o $outfile $hashlist $pwd_utils_dir/wordlists/worst/all_worst $pwd_utils_dir/wordlists/common/cat_all_common_words --force --remove -j $pwd_utils_dir/rules/cat_all_custom_rules.rule 
hashcat -m $hash_mode -a 1 -o $outfile $hashlist $pwd_utils_dir/wordlists/common/cat_all_common_words $pwd_utils_dir/wordlists/worst/all_worst --force --remove -j $pwd_utils_dir/rules/cat_all_custom_rules.rule
` 

## #10 Run brute-force using masks

If we have exhausted all the steps so far, the next step can be a brute-force attack using masks gathered from hashcat or generated using Pipal or Crunch or any other method you prefer. At this step I am also using a list with basewords meaning common substrings found in passwords when I ran Pipal over some lists with passwords and add some masks on top of those.

`
hashcat -m $hash_mode -a 1 -o $outfile $hashlist $pwd_utils_dir/wordlists/basewords/top_basewords $pwd_utils_dir/masks/crunch-generated --force --remove
hashcat -m $hash_mode -a 1 -o $outfile $hashlist $pwd_utils_dir/wordlists/worst/all_worst $pwd_utils_dir/masks/crunch-generated --force --remove
`

## #11 Run the exhaustive brute-force without masks

If everything failed, you can always run a brute-force attack on the specified length, meaning it will try to match all the possible combinations given an encoding type. If by this point your computer performance seemed acceptable, at this point you can find something to do that doesn’t involve your computer, for hours, days, weeks, depending on how many passwords you are trying to crack, what length did you specify, your computer performances and so on.

`
hashcat -m $hash_mode -a 3 -o $outfile $hashlist $pwd_utils_dir/masks/top_masks_less_than_8_chars --remove --force
hashcat -m $hash_mode -a 3 -o $outfile $hashlist $pwd_utils_dir/masks/top_masks_8_to_10_chars --remove --force
`

*All right, this is pretty much how you write a password cracking script. You will get a list with passwords that you want to crack, you will find out what hashing method was used and you will compile a collection with wordlists, masks and rules. After that, you put together all the command lines for each of the steps in a single script, run it and give it time and space to do its work while you enjoy your time. Hopefully this is enough to get you started and get your first password cracking results. ‘Till the next time!*