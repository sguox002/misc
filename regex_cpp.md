# C++ regex
regular expression defines a pattern to match. also for replace.
target sequence (subject)
regex: the pattern
match array: match information
replacement string:

regex is extremely powerful in searching and replacing a string.
only available since c++11.

c++ uses ECMAScript grammar:

range []:
[a-z] match one single lowercase letter
[0-9] match one single digit
[A-Za-z0-9] match one capital letter, or one lowercase letter or one digit
if you want to include [] in the search pattern, you need add \:
[\[0-9]: defines pattern to find [ or a digit.
in C++ you need use \\ to repreent the slash

Note: [] just search for a single character. In the [], it is or relation!

expression modifier:
used to modify previous pattern + and *
+: matching the pattern one or more times
*: matching the pattern 0 or more times
?: 0 or 1 times
{n}: match n times
{n,}: match n or more times
{m,n}: match m<=x<=n times
[a-z]+: match words
[a-z]*: 
(Xyz)+: will match Xyz, or XyzXyz..

### Special pattern characters
Special pattern characters are characters (or sequences of characters) that have a special meaning when they appear in a regular expression pattern, either to represent a character that is difficult to express in a string, or to represent a category of characters. Each of these special pattern characters is matched in the target sequence against a single character (unless a quantifier specifies otherwise).

.	not newline	any character except line terminators (LF, CR, LS, PS).
\t	tab (HT)	a horizontal tab character (same as \u0009).
\n	newline (LF)	a newline (line feed) character (same as \u000A).
\v	vertical tab (VT)	a vertical tab character (same as \u000B).
\f	form feed (FF)	a form feed character (same as \u000C).
\r	carriage return (CR)	a carriage return character (same as \u000D).
\cletter	control code	a control code character whose code unit value is the same as the remainder of dividing the code unit value of letter by 32.
For example: \ca is the same as \u0001, \cb the same as \u0002, and so on...
\xhh	ASCII character	a character whose code unit value has an hex value equivalent to the two hex digits hh.
For example: \x4c is the same as L, or \x23 the same as #.
\uhhhh	unicode character	a character whose code unit value has an hex value equivalent to the four hex digits hhhh.
\0	null	a null character (same as \u0000).
\int	backreference	the result of the submatch whose opening parenthesis is the int-th (int shall begin by a digit other than 0). See groups below for more info.
\d	digit	a decimal digit character (same as [[:digit:]]).
\D	not digit	any character that is not a decimal digit character (same as [^[:digit:]]).
\s	whitespace	a whitespace character (same as [[:space:]]).
\S	not whitespace	any character that is not a whitespace character (same as [^[:space:]]).
\w	word	an alphanumeric or underscore character (same as [_[:alnum:]]).
\W	not word	any character that is not an alphanumeric or underscore character (same as [^_[:alnum:]]).
\character	character	the character character as it is, without interpreting its special meaning within a regex expression.
Any character can be escaped except those which form any of the special character sequences above.
Needed for: ^ $ \ . * + ? ( ) [ ] { } |
[class]	character class	the target character is part of the class (see character classes below)
[^class]	negated character class	the target character is not part of the class (see character classes below)

### Quantifiers
Quantifiers follow a character or a special pattern character. They can modify the amount of times that character is repeated in the match:
characters	times	effects
*	0 or more	The preceding atom is matched 0 or more times.
+	1 or more	The preceding atom is matched 1 or more times.
?	0 or 1	The preceding atom is optional (matched either 0 times or once).
{int}	int	The preceding atom is matched exactly int times.
{int,}	int or more	The preceding atom is matched int or more times.
{min,max}	between min and max	The preceding atom is matched at least min times, but not more than max.
By default, all these quantifiers are greedy (i.e., they take as many characters that meet the condition as possible). This behavior can be overridden to ungreedy (i.e., take as few characters that meet the condition as possible) by adding a question mark (?) after the quantifier.
For example:
Matching "(a+).*" against "aardvark" succeeds and yields aa as the first submatch.
While matching "(a+?).*" against "aardvark" also succeeds, but yields a as the first submatch.
? can be used to turn off the greedy match.

### Groups
Groups allow to apply quantifiers to a sequence of characters (instead of a single character). There are two kinds of groups:
characters	description	effects
(subpattern)	Group	Creates a backreference.
(?:subpattern)	Passive group	Does not create a backreference.
When a group creates a backreference, the characters that represent the subpattern in the target sequence are stored as a submatch. Each submatch is numbered after the order of appearance of their opening parenthesis (the first submatch is number 1, the second is number 2, and so on...).

These submatches can be used in the regular expression itself to specify that the entire subpattern should appear again somewhere else (see \int in the special characters list). They can also be used in the replacement string or retrieved in the match_results object filled by some regex operations.

### Assertions
Assertions are conditions that do not consume characters in the target sequence: they do not describe a character, but a condition that must be fulfilled before or after a character.
characters	description	condition for match
^	Beginning of line	Either it is the beginning of the target sequence, or follows a line terminator.
$	End of line	Either it is the end of the target sequence, or precedes a line terminator.
\b	Word boundary	The previous character is a word character and the next is a non-word character (or vice-versa).
Note: The beginning and the end of the target sequence are considered here as non-word characters.
\B	Not a word boundary	The previous and next characters are both word characters or both are non-word characters.
Note: The beginning and the end of the target sequence are considered here as non-word characters.
(?=subpattern)	Positive lookahead	The characters following the assertion must match subpattern, but no characters are consumed.
(?!subpattern)	Negative lookahead	The characters following the assertion must not match subpattern, but no characters are consumed.

### Alternatives
A pattern can include different alternatives:
character	description	effects
|	Separator	Separates two alternative patterns or subpatterns.
A regular expression can contain multiple alternative patterns simply by separating them with the separator operator (|): The regular expression will match if any of the alternatives match, and as soon as one does.

### Character classes
A character class defines a category of characters. It is introduced by enclosing its descriptors in square brackets ([ and ]).
The regex object attempts to match the entire character class against a single character in the target sequence (unless a quantifier specifies otherwise).
The character class can contain any combination of:
Individual characters: Any character specified is considered part of the class (except the characters \, [, ] and - when they have a special meaning as described in the following paragraphs).
For example:
[abc] matches a, b or c.
[^xyz] matches any character except x, y and z.
Ranges: They can be specified by using the hyphen character (-) between two valid characters.
For example:
[a-z] matches any lowercase letter (a, b, c, ... until z).
[abc1-5] matches either a, b or c, or a digit between 1 and 5.
POSIX-like classes: A whole set of predefined classes can be added to a custom character class. There are three kinds:
class	description	notes
[:classname:]	character class	Uses the regex traits' isctype member with the appropriate type gotten from applying lookup_classname member on classname for the match.
[.classname.]	collating sequence	Uses the regex traits' lookup_collatename to interpret classname.
[=classname=]	character equivalents	Uses the regex traits' transform_primary of the result of regex_traits::lookup_collatename for classname to check for matches.
The choice of available classes depend on the regex traits type and on its selected locale. But at least the following character classes shall be recognized by any regex traits type and locale:
class	description	equivalent (with regex_traits, default locale)
[:alnum:]	alpha-numerical character	isalnum
[:alpha:]	alphabetic character	isalpha
[:blank:]	blank character	isblank
[:cntrl:]	control character	iscntrl
[:digit:]	decimal digit character	isdigit
[:graph:]	character with graphical representation	isgraph
[:lower:]	lowercase letter	islower
[:print:]	printable character	isprint
[:punct:]	punctuation mark character	ispunct
[:space:]	whitespace character	isspace
[:upper:]	uppercase letter	isupper
[:xdigit:]	hexadecimal digit character	isxdigit
[:d:]	decimal digit character	isdigit
[:w:]	word character	isalnum
[:s:]	whitespace character	isspace
Please note that the brackets in the class names are additional to those opening and closing the class definition.
For example:
[[:alpha:]] is a character class that matches any alphabetic character.
[abc[:digit:]] is a character class that matches a, b, c, or a digit.
[^[:space:]] is a character class that matches any character except a whitespace.
Escape characters: All escape characters described above can also be used within a character class specification. The only change is with \b, that here is interpreted as a backspace character (\u0008) instead of a word boundary.
Notice that within a class definition, those characters that have a special meaning in the regular expression (such as *, ., $) don't have such a meaning and are interpreted as normal characters (so they do not need to be escaped). Instead, within a class definition, the hyphen (-) and the brackets ([ and ]) do have special meanings under some circumstances, in which case they should be placed within the class in other locations where they do not have such special meaning, or be escaped with a backslash (\).

Character classes support depend heavily on the regex traits used by the regex object: the regex object calls its traits's isctype member function with the appropriate arguments. For the standard regex_traits object using the default locale, see cctype for a classification of characters.

Subpatterns (in groups or assertions) can also use the separator operator to separate different alternatives.

### example 1: strip cpp file comments
//... using \/\/.*$ matches the comment till end
```
abcd//this is comment...
```

/*anything*/ use \/\*.*\*\/ to match the substring in a line
```
code /*comments!!*/ codes
```
match multiple line comments:
```
code /* comments  
new code ...
abc*/

code();
//hello
/*start a new function*/
```
The example shall have two matches instead of one.
\/\*(.|[\r\n])*\*\/ this will greedily match from the first /* to the last */ which is incorrect.

between /* and */ we can have any number of non-space charater and/or any number of blank characters
to disable greedy:
use \/\*(.|[\r\n])*?\*\/ to match 0 or 1 only.

we can combine the two searches using separator |:
use: \/\/.*$|\/\*(.|[\r\n])*?\*\/

disable matching in a string:
for example:
```
str="this is a string with /*comment*/ and //others";
```

example 2:
given the raw:
```
#	Title	Solution	Acceptance	Difficulty	Frequency  
1161	
Maximum Level Sum of a Binary Tree    		71.0%	Medium	
1160	
Find Words That Can Be Formed by Characters    		67.3%	Easy	
1159	
Market Analysis II    		55.0%	Hard	
1158	
Market Analysis I    		63.0%	Medium	
1157	
Online Majority Element In Subarray    		39.3%	Hard	
1156	
Swap For Longest Repeated Character Substring    		47.7%	Medium	
1155	
Number of Dice Rolls With Target Sum    		47.8%	Medium	
1154	
Day of the Year    		49.3%	Easy	
```
we want to put the whole record into one single line.
search for number followed by \r\n and strip the \r\n
(^\d+\s+)\r\n
and replaced with $1:
The output would be:
```
#	Title	Solution	Acceptance	Difficulty	Frequency  
1161	Maximum Level Sum of a Binary Tree    		71.0%	Medium	
1160	Find Words That Can Be Formed by Characters    		67.3%	Easy	
1159	Market Analysis II    		55.0%	Hard	
1158	Market Analysis I    		63.0%	Medium	
1157	Online Majority Element In Subarray    		39.3%	Hard	
1156	Swap For Longest Repeated Character Substring    		47.7%	Medium	
1155	Number of Dice Rolls With Target Sum    		47.8%	Medium	
1154	Day of the Year    		49.3%	Easy	
```

### using regex in C++

regex(reg_ex) to build a regex search pattern
can have options:
icase: ignore case
nosubs
optimize
collate
ecmascript
basic
extended
awk
grep
egrep

regex_search(text,regex_obj): using the regex search pattern to do search in the text.
regex_search return true or false.
it has several forms of overloading functions:
You can get match_results object:
```
match_result res=smatch();
regex_search(text,res,regex_obj)
```
reg_match: check if the whole string matches the pattern. often used for validation of input.

using slash \:
special meaning char need to be slashed.

to avold a lot of slashes, we can use c++11 raw string literals
regex(R"((\bmy\b|\byour\b) regex)");















