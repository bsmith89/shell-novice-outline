## Instructor Setup ##

-   Move `.bash_aliases` to `.bash_aliases~`.
-   Make sure that students have downloaded the data files and unpacked them
    on their Desktop as `data-shell`.
-   Erase the prompt: `PS1=""`.
-   Start the Socrative quiz and ask students to join it:
    <https://b.socrative.com/login/student/>

## Files and Directories ##

-   `whoami`
-   A command a single word which we type in and press `<ENTER>`.
-   `pwd`
-   The present working directory may not look the same for everyone
-   `ls`
-   `ls -F`
-   `-F` is a "flag" which changes the behavior of `ls`
-   `ls -F Desktop`
-   `Desktop` directs `ls` to print the contents of a directory besides the
    current one
-   `ls -F Desktop/data-shell`
-   Here we've combined a directory argument with a sub-directory using `/`.
    Read this as list the contents of `data-shell` which is inside `Desktop`.

-   `cd Desktop`
-   `cd data-shell`
-   `cd data`
-   Step down iteratively until you're inside `Desktop/data-shell/data`.
-   `cd data-shell  # This fails`
-   How do we get back to `data-shell`???
-   `cd ..`
-   `..` is a special directory which means, one directory above where I currently
    am.
-   `pwd` prints `/Users/bjsmith/Desktop/data-shell/`, so `cd ..` would put
    me into `Desktop/`.
-   `ls -F -a`
-   Notice that we are now listing additional files which didn't show up before
    -   Prefixing a `.` _hides_ a file
    -   Notice a directory called `./` and a file call `.bash_profile`.

-   Socrative Question #2 (`ls ../backup`)

-   Socrative Question #3 (`cd  # without arguments`)

-   `cd Desktop/data-shell/data`
-   `cd /Users/nelle/Desktop/data-shell`
-   `cd ~/Desktop/data-shell`

-   Socrative Question #4 (Many ways to get around)


-   Describe the organization of Nelle's computer.
    -   Data inside a directory for the project and then a directory for the
        date that the data was collected.
    -   The date is written `YYYY-MM-DD` with 4-digit year first, and leading zeros
-   `cd ~/Desktop/data-shell/nor<TAB>`  Tab completion.

-   Socrative Question #5 (Reverse order `ls`; Challenge)


## Creating/Moving Files/Directories ##

-   (`cd ~/Desktop/data-shell`)

-   `mkdir thesis`  (show `ls -F` before and after)
-   `cd thesis`
-   `nano draft.txt` (some folks may have trouble with nano)
    Write a first draft of your thesis
-   Save (`<CTRL>-O`) and exit (`<CTRL>-X`)
-   `rm draft.txt`
-   `cd ..`
-   `rm thesis/  # This fails`
-   `rmdir thesis`
-   `nano thesis/draft.txt  # and write a new draft`
-   `mv thesis/draft.txt thesis/quotes.txt`
-   Careful, you might accidentally overwrite an existing file

-   Socrative Question #6 (Renaming a file)

-   `mv thesis/quotes.txt .`
-   `cp quotes.txt thesis/quotations.txt`
-   `ls`
-   `ls thesis`
-   `rm quotes.txt`
-   `ls quotes.txt thesis/quotations.txt`
-   Talk about why we name files the way we do (extensions)

-   Socrative Question #7 (Mental model of dir structure)

-   Ask students to figure out the answer to these questions:

    What does cp do when given several filenames and a directory name, as in:

    ```
    $ mkdir backup
    $ cp thesis/citations.txt thesis/quotations.txt backup
    ```

    What does cp do when given three or more filenames, as in:

    ```
    $ ls -F
    intro.txt    methods.txt    survey.txt
    $ cp intro.txt methods.txt survey.txt
    ```


**(TAKE A BREAK!!!!)**



## Pipes and Filters ##

-   (`cd ~/Desktop/data-shell/data/molecules`)
-   (`ls .`)
-   `wc *.pdb`
-   Talk ab/t wildcards, including `?`

-   Socrative Question #8 (wildcards)

-   `wc -l *.pdb`
-   We want to know which file has the fewest lines.
    How do we do this when we have a _very large_ number of files?
-   `wc -l *.pdb > lengths.txt`
-   Talk ab/t using the history (`<UP>`)
-   The `>` great-than symbol says "take the output of this command and put
    it into a file".
-   `cat lengths.txt`
-   Alternatively `less lengths.txt`
-   `sort -n lengths.txt`
-   The `-n` flag sorts numerically, so 9 comes before 12.
-   `sort -n lengths.txt > sorted-lengths.txt`
-   `head -n 1 sorted-lengths.txt`
-   Head takes the top N lines from a file.  Default 10.
-   `sort -n lengths.txt | head -n 1`
-   Here we've used the `|` (pronounced "pipe") to take the output of `sort`
    and use it as the input to `head`.
    -   It's just like we made `sorted-lengths.txt` without actually making it.
-   `wc -l *.pdf | sort -n | head -n 1`
-   Think of it as a conveyor belt.  Something comes in to the first command
    (`wc`) and something new comes out.  This new thing goes into the next
    command (`sort`) which spits out something new, and this final intermediate
    goes into `head` which produces the answer.
-   Instead of creating a file in between, we let the computer deal with
    keeping track of the data.
-   The input to a program is called `stdin` and the output `stdout`.
-   When you use a pipe, you "redirect" one program's `stdout` to another
    program's `stdin`.

-   Socrative Question #9 (pipeline syntax)

-   Socrative Question #10 (append with `>>`)

-   Each of the programs which takes something to standard-in and produces
    something to standard-out is called a "filter".  Composing filters into long pipelines is a powerful way of processing
    data.
-   UNIXy programs are single-purpose and generally useful, which makes
    it easier to reason about computation.

-   Socrative Question #11 (`uniq` only removes adjacent)

-   Talk ab/t `man`, googling

-   Talk about how Nelle might sanity check here `north-pacific-gyre/2012-07-03`
    data (using `wc -l`).

-   Challenge question!
    -   Both `molecules/` and `data/pdb` contain files in the `.pdb` format.
    -   Design a way for Nelle to determine if any molecules are in both
        directories.


## Loops ##

-   (`cd creatures/`)

-   Let's say we want to remake `basilisk.dat` and `unicorn.dat`, but
    keep backups of the original files.
-   `cp *.dat original-*.dat   # error!`
-   We could `cp basilisk.dat original-basilisk.dat` and
    `cp unicorn.dat original-unicorn.dat` but this doesn't work so well
    when we have hundreds of files (think "big data")
-   Use a **loop**

    ```bash
    for filename in basilisk.dat unicorn.dat
    do
        head -n 3 $filename
    done
    ```

-   Explain the syntax, the prompt, loop variables, logic, etc.
-   Explain using variables (the shell doesn't care what loop variable we use)
-   More complicated loop

    ```bash
    for filename in *.dat
    do
        echo $filename
        head -n 100 $filename | tail -n 10
    done
    ```

-   Socrative Question #12 (globbing inside a loop)

-   Use a loop to copy the two files to their new names
    -   Show how to check the output by `echo`-ing the command

-   Write loop to do `bash goostats` on each file in `north-pacific-gyre/2012-07-03`.
-   Discuss up-arrow for history

-   Socrative Question #13 (nested loops)


**(Take a quick break)**


## Shell Scripts ##

-   (`cd molecule/`)
-   Remind students of `bash goostats`
-   `nano middle.sh`
    -   `head -n 15 octane.pdb | tail -n 5`
    -   `bash middle.sh`
-   edit `middle.sh`
    -   `head -n 15 "$1" | tail -n 5`
    -   `bash middle.sh octane.pdb`
-   Discuss whitespace and (double-)quoting
-   Discuss `history` command; reproducibility
-   Add a documentation comment to the top of the script
