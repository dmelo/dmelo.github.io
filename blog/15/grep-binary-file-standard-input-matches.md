

# grep -- Binary file (standard input) matches

When grep finds your match on a binary input, instead of printing the matches, it will just tell you that there was a hit.

If you want to see the matches, uses the option "-a". Example:

    cat file | grep -a
