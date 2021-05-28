title: bash sequence expression (range)
date: 2021-04-10
tags: [bash, shell]

# Bash sequence expression (range)

In this article, we will cover the basics of the sequence expression in Bash.

The Bash sequence expression generates a range of integers or characters by defining a start and the end point of the range. It is generally used in combination with `for` loops.

## Bash Sequence Expression

The sequence expression takes the following form:

```
{START..END[..INCREMENT]}
```

* The expression begins with an opening brace and ends with a closing brace.
* `START` and `END` can be either positive integers or single characters.
* The `START` and the `END` values are mandatory and separated with two dots `..`, with no space between them.
* The `INCREMENT` value is optional. If present, it must be separated from the `END` value with two dots `..`, with no space between them. When characters are given, the expression is expanded in lexicographic order.
* The expression expands to each number or characters between `START` and `END`, including the provided values.
* An incorrectly formed expression is left unchanged.

Here’s the expression in action:

```bash
echo {0..3}
```

When no `INCREMENT` is provided the default increment is 1:

```bash
0 1 2 3
```

You can also use other characters. The example below prints the alphabet:

```bash
echo {a..z}
```

```bash
a b c d e f g h i j k l m n o p q r s t u v w x y z
```

If the `START` value is greater than `END` then the expression will create a range that decrements:

```bash
for i in {3..0}
do
  echo "Number: $i"
done
```

```bash
Number: 3
Number: 2
Number: 1
Number: 0
```

When an `INCREMENT` is given, it is used as the step between each generated item:

```bash
for i in {0..20..5}
do
  echo "Number: $i"
done
```

Each generated number is greater than the preceding number by 5:

```bash
Number: 0
Number: 5
Number: 10
Number: 15
Number: 20
```

When using integers to generate a range, you can add a leading `0` to force each number to have the same length. To pad generated integers with leading zeros prefix either `START` and `END` with a zero:

```bash
for i in {00..3}
do
  echo "Number: $i"
done
```

```bash
Number: 00
Number: 01
Number: 02
Number: 03
```

The expression can be prefixed or suffixed with other characters:

```bash
echo A{00..3}B
```

```bash
A00B A01B A02B A03B
```

If the expression is not constructed correctly, it is left unchanged:

```bash
echo {0..}
```

```bash
0..
```

## Conclusion

The Bash sequence expression allows you to generate a range of integers or characters.
