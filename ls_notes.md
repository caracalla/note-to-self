# UNIX Version 7 ls.c Notes

How does `ls` work on UNIX version 7? Let's find out.

Source: https://minnie.tuhs.org/cgi-bin/utree.pl?file=V7/usr/src/cmd/ls.c

## External Variables
```
struct lbuf {
  union {
    char lname[15];
    char *namep;
  } ln;
  char ltype;
  short lnum;
  short lflags;
  short lnl;
  short luid;
  short lgid;
  long lsize;
  long lmtime;
}
```

```c
FILE *pwdf
FILE *dirf
char stdbuf[] - setbuf(stdout, stdbuf)
long year
int	aflg, dflg, lflg, sflg, tflg, uflg, iflg, fflg, gflg, cflg
int flags
int lastuid
char tbuf[]
long tblocks
int statreq
struct lbuf *flist[1024]
struct lbuf **lastp = flist
struct lbuf **firstp = flist
char *dotp = "."
```

## Functions
```c
char *makename()
struct lbuf *gstat()
char *ctime()
long nblock()
int compar()
```

### main
```c
int i
struct lbuf *ep
struct lbuf **ep1
struct lbuf **epp
struct lbuf lb
char *t
```

stores unix time in `lb.lmtime`
stores `lb.lmtime - 6L*30L*24L*60L*60L` in `year` - `/* 6 months ago */`
parses argv:
  decrements argc to check if it's greater than one
  checks if argv[1] == '-'
  increments argv
