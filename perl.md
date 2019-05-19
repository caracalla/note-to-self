# Perl
my - local scope variable specifier
$ - scalar variable (numbers and strings)
@ - array
% - hash

## Arrays
@

###indexing:
```perl
my @array = (1, 2, 3);
my $value = $array[0];
# OR
$value = (@array)[0];
my $index = $#array; # the index of the last element in the array

for my $element (@array) {
  # use $element - $element is a reference!
}

for (@array) {
  # use $_ as array element
}

for (0..$#array) {
  # use $_ as array index
}
```

### methods
reverse, push, pop, shift, unshift

sort:
    ```perl
    # sorting numbers is weird
    my @sorted_numbers = sort {$a <=> $b} @unsorted_numbers;
    ```

## Hashes
%
```perl
%hash = (
  this => "that",
  # OR
  "here", "there"
);
```

### indexing:
```perl
my $value = $hash{this}; # "that"
```

### methods
keys:
    ```perl
    for (keys $hash) {
      # use $_ as key
    }
    ```

values

## Reading input
```perl
my $input = <STDIN>;
chomp($input); # destructively removes newline if there is one
```

## Program control
```perl
my %hash = {this => "that", those => "these"};
my $input = <STDIN>;
chomp($input);

if (not exists $hash{input}) { # OR unless (exists $hash{input}) {}
  die "error message\n";
}
OR
die "error message\n" if (not exists $hash{input});
```

if, elsif, else

0, "", "0", (), and undefined variables are falsey
for strings, use `gt`, `le`, `eq`, `ne` for comparison

### while
while, do-while, and until

```perl
while (<STDIN>) {
  chomp;
  die "end of input" unless $_;
  # ...
}
# OR
my $bool = 0;
until ($bool) {
  my $input = <STDIN>;
  chomp($input);
  if ($input) {
    # ...
  } else {
    $bool = 1;
  }
}
```

`last` == break
`next`

## References
```perl
my $array_ref = \@array;
my $array_ref = [1, 2, 3, 4];
my $hash_ref = \%hash;
my $hash_ref = {this => "that", those => "these"};
```

allows putting lists/hashes inside lists/hashes:
    ```perl
    my %thing = (
      thing1 => [1, 2, 3, 4],
      thing2 => qw(one two three four)
    );
    ```

### dereference
`@{$array_ref}` or `%{$hash_ref}`
The brackets aren't even strictly necessary

```perl
my @array = (1, 2, 3, 4);
my $array_ref = \@array;

for (@{$array_ref}) {
  # ...
}
```

```perl
${$array_ref}[0];
# is the same as
$array_ref->[0];
```

## Subroutines

```perl
sub first_line_of_file {
  my $filename = shift;
  open FILE, $filename or return "";
  my $line = <FILE>;
  return $line;
}
```

specify the number of arguments by dollar signs:
    ```perl
    sub my_sub($$$) { ... } # takes 3 arguments
    ```

## Command Line Arguments
```perl
my ($arg1, $arg2) = @ARGV;
```

## Scope
globals, packages: `$main::variable`, `our`, `use`
lexical scope - `my`
runtime scope - `local $_` (don't use)
