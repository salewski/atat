# atat

The `atat` project provides a simple script that wraps the `at(1)` command to
pretty-print the output of:
```
    $ at -l
```

In particular, it allows the `at(1)` jobs listing to be sorted in the order in
which the jobs will be executed, which is something that is not easily done by
default output.

## Example:
```
    $ atat
    48 2020-04-24 03:00:00 a ads
    47 2020-05-25 03:00:00 a ads
    39 2020-06-24 03:00:00 a ads
    41 2020-07-25 03:00:00 a ads
    51 2020-08-25 03:00:00 a ads
    49 2020-09-24 03:00:00 a ads
    45 2020-10-25 03:00:00 a ads
    43 2020-11-24 03:00:00 a ads
    37 2020-12-20 03:00:00 a ads
    50 2021-01-25 03:00:00 a ads
    40 2021-02-22 03:00:00 a ads
    52 2021-03-25 03:00:00 a ads
    44 2021-04-24 03:00:00 a ads
    38 2021-05-25 03:00:00 a ads
    36 2021-06-24 03:00:00 a ads
    46 2021-07-25 03:00:00 a ads
    42 2021-08-25 03:00:00 a ads
```

The default output of `atat(1)` (shown above) sorts the `at(1)` jobs by the
order in which they will be executed, and then by job number.

Note that (unlike the default output of `at(1)`) the `atat(1)` output is
easily sorted by other criteria, as well, though. For example, to sort by
queue name, then by user, then by job sequence, then by job number one could
do this:
```
    $ atat | sort -k4,4 -k5,5 -k2,2 -k3,3 -k1,1
```

# Motivation

The output of the `at -l` command is not sorted by job execution sequence, and
the default date and time output it produces is difficult to work with.

For example, consider the default output of `at -l` (a.k.a. `atq(1)`):
```
    $ at -l
    39      Wed Jun 24 03:00:00 2020 a ads
    37      Sun Dec 20 03:00:00 2020 a ads
    49      Thu Sep 24 03:00:00 2020 a ads
    46      Sun Jul 25 03:00:00 2021 a ads
    36      Thu Jun 24 03:00:00 2021 a ads
    42      Wed Aug 25 03:00:00 2021 a ads
    38      Tue May 25 03:00:00 2021 a ads
    50      Mon Jan 25 03:00:00 2021 a ads
    45      Sun Oct 25 03:00:00 2020 a ads
    44      Sat Apr 24 03:00:00 2021 a ads
    48      Fri Apr 24 03:00:00 2020 a ads
    47      Mon May 25 03:00:00 2020 a ads
    52      Thu Mar 25 03:00:00 2021 a ads
    43      Tue Nov 24 03:00:00 2020 a ads
    41      Sat Jul 25 03:00:00 2020 a ads
    40      Mon Feb 22 03:00:00 2021 a ads
    51      Tue Aug 25 03:00:00 2020 a ads
```

Though those could be easily sorted by the job number in the first column, the
result is less useful than it might otherwise be unless the primary concern is
somehow related to the number and/or values of the currently outstanding list
of jobs. The below example makes it easy to see the number of outstanding
jobs, and by quick inspection the lowest and highest job numbers can be
determined. However, note that the sequence in which the jobs will be executed
is not easily discernible, and if we are in for some tedium if we want to sort
by some other criteria:
```
    $ at -l | sort -k1,1n
    36      Thu Jun 24 03:00:00 2021 a ads
    37      Sun Dec 20 03:00:00 2020 a ads
    38      Tue May 25 03:00:00 2021 a ads
    39      Wed Jun 24 03:00:00 2020 a ads
    40      Mon Feb 22 03:00:00 2021 a ads
    41      Sat Jul 25 03:00:00 2020 a ads
    42      Wed Aug 25 03:00:00 2021 a ads
    43      Tue Nov 24 03:00:00 2020 a ads
    44      Sat Apr 24 03:00:00 2021 a ads
    45      Sun Oct 25 03:00:00 2020 a ads
    46      Sun Jul 25 03:00:00 2021 a ads
    47      Mon May 25 03:00:00 2020 a ads
    48      Fri Apr 24 03:00:00 2020 a ads
    49      Thu Sep 24 03:00:00 2020 a ads
    50      Mon Jan 25 03:00:00 2021 a ads
    51      Tue Aug 25 03:00:00 2020 a ads
    52      Thu Mar 25 03:00:00 2021 a ads
```
