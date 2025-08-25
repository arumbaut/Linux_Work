The sort command arranges text lines in useful ways. This simple tool can help you quickly sort information from the command line.

**Syntax**

```
sort [options] <filename>
```

You should note a few thing:

- When you use sort without any options, the default rules are enforced. It helps to understand the default rules to avoid unexpected outcomes.
- When using sort, your original data is safe. The results of your input are displayed on the command line only. However, you can specify output to a separate file if you wish. More on that later.
- Sort was originally designed for use with ASCII characters. I did not test for this, but it is possible that different encodings may produce unexpected results.

**The default rules in the sort command**

These are the default rules when using sort. The first few examples will clarify how these priorties are managed. Then we will look at specialized options.

- numbers > letters
- lowercase > uppercase

## Examples of the sort command

Let me show you some examples of sort command that you can use in various situations.

### 1\. Sort in alphabetical order

The default sort command makes it easy to view information in alphabetical order. No options are necessary and even with mixed-case entries, A-Z sorting works as expected.

I am going to use a sample text file named filename.txt and if you [view the content of the file](https://linuxhandbook.com/view-file-linux/), this is what you’ll see:

```
MX Linux
Manjaro
Mint
elementary
Ubuntu
```

Now if you use sort command on it:

```
sort filename.txt
```

Here’s the alphabetically sorted output:

```
elementary
Manjaro
Mint
MX Linux
Ubuntu
```

### 2\. Sort on numerical value \[option -n\]

Let’s take the same list we used for the previous example and sort in numerical order. In case you were wondering, the list reflects the most popular Linux distributions (July, 2019) according to [distrowatch.com](https://distrowatch.com/).

I will modify the contents of the file so that the items are numbered, but out of order as shown below.

```
1. MX Linux
4. elementary
2. Manjaro
5. Ubuntu
3. Mint
```
```
sort filename.txt
```

After sorting, the result is:

```
1. MX Linux
2. Manjaro
3. Mint
4. elementary
5. Ubuntu
```

Looks good, right? Can you rely on this method to arrange your data accurately, though? *Probably not.* Let’s look at another example to find out why.

Here’s my new sample text:

```
1
5
10
3
5
2
60
23
432
21
```

Now, if I use the sort command without any options, here’s what I get:

```
chris@discodingo:~$ sort order.txt
```
```
1
10
2
21
23
3
432
5
5
60
```

📋

Numbers are sorted by their leading characters only.

When you add the `-n` option, the numerical value of the string is now being evaluated rather than only the first character. Now, you can see below that our list is properly sorted.

```
sort order.txt -n
```

Now you’ll have the correctly sorted output:

```
1
2
3
5
5
10
21
23
60
432
```

### 3\. Sort in reverse order \[option -r\]

For this one, I am going to use our distro list again. The reverse function is self-explanatory. It will reverse the order of whatever content you have in your file.

```
sort filename.txt -r
```

And here you have the output text in reverse order:

```
5. Ubuntu
4. elementary
3. Mint
2. Manjaro
1. MX Linux
```

### 4\. Random sort \[option -R\]

If you accidentally mashed your shift key while attempting the reverse function, you might have gotten some strange results. `-R` rearranges output in randomized order.

```
sort filename.txt -R
```

Here’s the randomly sorted output:

```
4. elementary
1. MX Linux
2. Manjaro
5. Ubuntu
3. Mint
```

### 5\. Sort by months \[option -M\]

Sort also has built in functionality to arrange by month. It recognizes several formats based on locale-specific information. I tried to demonstrate some unqiue tests to show that it will arrange by date-day, but not year. Month abbreviations display before full-names.

Here is the sample text file in this example:

```
March
Feb
February
April
August
July
June
November
October
December
May
September
1
4
3
6
01/05/19
01/10/19
02/06/18
```

Let’s sort it by months using the -M option:

```
sort filename.txt -M
```

Here’s the output you’ll see:

```
01/05/19
01/10/19
02/06/18
1
3
4
6
Jan
Feb
February
March
April
May
June
July
August
September
October
November
December
```

### 6\. Save the sorted results to another file

As I mentioned earlier, sort does not change the original file by default. If you need to save the sorted content, it can be done.

For this example, I’ve created a new file where I want the sorted information to be printed and saved with the name filename\_sorted.txt.

***Caution:* If you try to direct your sorted data to the same file, it will erase the contents of your file.**

```
sort filename.txt -n > filename_sorted.txt
```

If you [use cat command](https://linuxhandbook.com/cat-command/) on the output file, this will be its contents:

```
1. MX Linux
2. Manjaro
3. Mint
4. elementary
5. Ubuntu
```

### 7\. Sort Specific Column \[option -k\]

If you have a table in your file, you can use the `-k` option to specify which column to sort. I added some arbitrary numbers as a third column and will display the output sorted by each column. I’ve included several examples to show the variety of output possible. Options are added following the column number.

```
1. MX Linux 100
2. Manjaro 400
3. Mint 300
4. elementary 500
5. Ubuntu 200
```
```
sort filename.txt -k 2
```

This will sort the text on the second column in alphabetical order:

```
4. elementary 500
2. Manjaro 400
3. Mint 300
1. MX Linux 100
5. Ubuntu 200
```
```
sort filename.txt -k 3n
```

This will sort the text by the numerals on the third column.

```
1. MX Linux 100
5. Ubuntu 200
3. Mint 300
2. Manjaro 400
4. elementary 500
```
```
sort filename.txt -k 3nr
```

Same as the above command just that the sort order has been reversed.

```
4. elementary 500
2. Manjaro 400
3. Mint 300
5. Ubuntu 200
1. MX Linux 100
```

### 8\. Sort and remove duplicates \[option -u\]

If you have a file with potential duplicates, the `-u` option will make your life much easier. Remember that sort will not make changes to your original data file. I chose to create a new file with just the items that are duplicates. Below you’ll see the input and then the contents of each file after the command is run.

```
1. MX Linux
2. Manjaro
3. Mint
4. elementary
5. Ubuntu
1. MX Linux
2. Manjaro
3. Mint
4. elementary
5. Ubuntu
1. MX Linux
2. Manjaro
3. Mint
4. elementary
5. Ubuntu
```
```
sort filename.txt -u > filename_duplicates.txt
```

Here’s the output files sorted and without duplicates.

```
1. MX Linux 
2. Manjaro 
3. Mint 
4. elementary 
5. Ubuntu
```

### 9\. Ignore case while sorting \[option -f\]

Many modern distros running sort will implement ignore case by default. If yours does not, adding the `-f` option will produce the expected results.

```
sort filename.txt -f
```

Here’s the output where cases are ignored by the sort command:

```
alpha
alPHa
Alpha
ALpha
beta
Beta
BEta
BETA
```

### 10\. Sort by human numeric values \[option -h\]

This option allows the comparison of alphanumeric values like 1k (i.e. 1000).

```
sort filename.txt -h
```

Here’s the sorted output:

```
10.0
100
1000.0
1k
```

I hope this tutorial helped you get the basic usage of the sort command in Linux. Sort command is often used in conjugation with the [uniq command in Linux](https://linuxhandbook.com/uniq-command/) for uniquely sorting text files.

If you have some cool sort trick, why not share it with us in the comment section?

[

Previous \- File Manipulation Commands tr Command Examples

](https://linuxhandbook.com/tr-command/)[

Next

](https://linuxhandbook.com/uniq-command/)

Christopher works as a Software Developer in Orlando, FL. He loves open source, Taco Bell, and a Chi-weenie named Max.

USA