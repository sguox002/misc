# C++ regex</br>

regular expression defines a pattern to match. also for replace.</br>
target sequence (subject)</br>
regex: the pattern</br>
match array: match information</br>
replacement string:</br>
</br>
regex is extremely powerful in searching and replacing a string.</br>
only available since c++11.</br>
</br>
c++ uses ECMAScript grammar:</br>
special character: []().^$*?+|\
some flavor: {}
range []:</br>
[a-z] match one single lowercase letter</br>
[0-9] match one single digit</br>
[A-Za-z0-9] match one capital letter, or one lowercase letter or one digit</br>
if you want to include [] in the search pattern, you need add \:</br>
[\[0-9]: defines pattern to find [ or a digit.</br>
in C++ you need use \\ to repreent the slash</br>
</br>
Note: [] just search for a single character. In the [], it is or relation!</br>
</br>
expression modifier:</br>
used to modify previous pattern + and *</br>
+: matching the pattern one or more times</br>
*: matching the pattern 0 or more times</br>
?: 0 or 1 times</br>
{n}: match n times</br>
{n,}: match n or more times</br>
{m,n}: match m<=x<=n times</br>
[a-z]+: match words</br>
[a-z]*: </br>
(Xyz)+: will match Xyz, or XyzXyz..</br>
</br>
### Special pattern characters</br>
Special pattern characters are characters (or sequences of characters) that have a special meaning when they appear in a regular expression pattern, either to represent a character that is difficult to express in a string, or to represent a category of characters. Each of these special pattern characters is matched in the target sequence against a single character (unless a quantifier specifies otherwise).</br>
</br>
.	not newline	any character except line terminators (LF, CR, LS, PS).</br>
\t	tab (HT)	a horizontal tab character (same as \u0009).</br>
\n	newline (LF)	a newline (line feed) character (same as \u000A).</br>
\v	vertical tab (VT)	a vertical tab character (same as \u000B).</br>
\f	form feed (FF)	a form feed character (same as \u000C).</br>
\r	carriage return (CR)	a carriage return character (same as \u000D).</br>
\cletter	control code	a control code character whose code unit value is the same as the remainder of dividing the code unit value of letter by 32.</br>
For example: \ca is the same as \u0001, \cb the same as \u0002, and so on...</br>
\xhh	ASCII character	a character whose code unit value has an hex value equivalent to the two hex digits hh.</br>
For example: \x4c is the same as L, or \x23 the same as #.</br>
\uhhhh	unicode character	a character whose code unit value has an hex value equivalent to the four hex digits hhhh.</br>
\0	null	a null character (same as \u0000).</br>
\int	backreference	the result of the submatch whose opening parenthesis is the int-th (int shall begin by a digit other than 0). See groups below for more info.</br>
\d	digit	a decimal digit character (same as [[:digit:]]).</br>
\D	not digit	any character that is not a decimal digit character (same as [^[:digit:]]).</br>
\s	whitespace	a whitespace character (same as [[:space:]]).</br>
\S	not whitespace	any character that is not a whitespace character (same as [^[:space:]]).</br>
\w	word	an alphanumeric or underscore character (same as [_[:alnum:]]).</br>
\W	not word	any character that is not an alphanumeric or underscore character (same as [^_[:alnum:]]).</br>
\character	character	the character character as it is, without interpreting its special meaning within a regex expression.</br>
Any character can be escaped except those which form any of the special character sequences above.</br>
Needed for: ^ $ \ . * + ? ( ) [ ] { } |</br>
[class]	character class	the target character is part of the class (see character classes below)</br>
[^class]	negated character class	the target character is not part of the class (see character classes below)</br>
</br>

### Quantifiers</br>
Quantifiers follow a character or a special pattern character. They can modify the amount of times that character is repeated in the match:</br>
characters	times	effects</br>
*	0 or more	The preceding atom is matched 0 or more times.</br>
+	1 or more	The preceding atom is matched 1 or more times.</br>
?	0 or 1	The preceding atom is optional (matched either 0 times or once).</br>
{int}	int	The preceding atom is matched exactly int times.</br>
{int,}	int or more	The preceding atom is matched int or more times.</br>
{min,max}	between min and max	The preceding atom is matched at least min times, but not more than max.</br>
By default, all these quantifiers are greedy (i.e., they take as many characters that meet the condition as possible). This behavior can be overridden to ungreedy (i.e., take as few characters that meet the condition as possible) by adding a question mark (?) after the quantifier.</br>
For example:</br>
Matching "(a+).*" against "aardvark" succeeds and yields aa as the first submatch.</br>
While matching "(a+?).*" against "aardvark" also succeeds, but yields a as the first submatch.</br>
? can be used to turn off the greedy match.</br>
</br>

### Groups</br>
Groups allow to apply quantifiers to a sequence of characters (instead of a single character). There are two kinds of groups:</br>
characters	description	effects</br>
(subpattern)	Group	Creates a backreference.</br>
(?:subpattern)	Passive group	Does not create a backreference.</br>
When a group creates a backreference, the characters that represent the subpattern in the target sequence are stored as a submatch. Each submatch is numbered after the order of appearance of their opening parenthesis (the first submatch is number 1, the second is number 2, and so on...).</br>
</br>
These submatches can be used in the regular expression itself to specify that the entire subpattern should appear again somewhere else (see \int in the special characters list). They can also be used in the replacement string or retrieved in the match_results object filled by some regex operations.</br>
</br>

### Assertions</br>
Assertions are conditions that do not consume characters in the target sequence: they do not describe a character, but a condition that must be fulfilled before or after a character.</br>
characters	description	condition for match</br>
^	Beginning of line	Either it is the beginning of the target sequence, or follows a line terminator.</br>
$	End of line	Either it is the end of the target sequence, or precedes a line terminator.</br>
\b	Word boundary	The previous character is a word character and the next is a non-word character (or vice-versa).</br>
Note: The beginning and the end of the target sequence are considered here as non-word characters.</br>
\B	Not a word boundary	The previous and next characters are both word characters or both are non-word characters.</br>
Note: The beginning and the end of the target sequence are considered here as non-word characters.</br>
(?=subpattern)	Positive lookahead	The characters following the assertion must match subpattern, but no characters are consumed.</br>
(?!subpattern)	Negative lookahead	The characters following the assertion must not match subpattern, but no characters are consumed.</br>
</br>

### Alternatives</br>
A pattern can include different alternatives:</br>
character	description	effects</br>
|	Separator	Separates two alternative patterns or subpatterns.</br>
A regular expression can contain multiple alternative patterns simply by separating them with the separator operator (|): The regular expression will match if any of the alternatives match, and as soon as one does.</br>
</br>

### Character classes</br>
A character class defines a category of characters. It is introduced by enclosing its descriptors in square brackets ([ and ]).</br>
The regex object attempts to match the entire character class against a single character in the target sequence (unless a quantifier specifies otherwise).</br>
The character class can contain any combination of:</br>
Individual characters: Any character specified is considered part of the class (except the characters \, [, ] and - when they have a special meaning as described in the following paragraphs).</br>
For example:</br>
[abc] matches a, b or c.</br>
[^xyz] matches any character except x, y and z.</br>
Ranges: They can be specified by using the hyphen character (-) between two valid characters.</br>
For example:</br>
[a-z] matches any lowercase letter (a, b, c, ... until z).</br>
[abc1-5] matches either a, b or c, or a digit between 1 and 5.</br>
POSIX-like classes: A whole set of predefined classes can be added to a custom character class. There are three kinds:</br>
class	description	notes</br>
[:classname:]	character class	Uses the regex traits' isctype member with the appropriate type gotten from applying lookup_classname member on classname for the match.</br>
[.classname.]	collating sequence	Uses the regex traits' lookup_collatename to interpret classname.</br>
[=classname=]	character equivalents	Uses the regex traits' transform_primary of the result of regex_traits::lookup_collatename for classname to check for matches.</br>
The choice of available classes depend on the regex traits type and on its selected locale. But at least the following character classes shall be recognized by any regex traits type and locale:</br>
class	description	equivalent (with regex_traits, default locale)</br>
[:alnum:]	alpha-numerical character	isalnum</br>
[:alpha:]	alphabetic character	isalpha</br>
[:blank:]	blank character	isblank</br>
[:cntrl:]	control character	iscntrl</br>
[:digit:]	decimal digit character	isdigit</br>
[:graph:]	character with graphical representation	isgraph</br>
[:lower:]	lowercase letter	islower</br>
[:print:]	printable character	isprint</br>
[:punct:]	punctuation mark character	ispunct</br>
[:space:]	whitespace character	isspace</br>
[:upper:]	uppercase letter	isupper</br>
[:xdigit:]	hexadecimal digit character	isxdigit</br>
[:d:]	decimal digit character	isdigit</br>
[:w:]	word character	isalnum</br>
[:s:]	whitespace character	isspace</br>
Please note that the brackets in the class names are additional to those opening and closing the class definition.</br>
For example:</br>
[[:alpha:]] is a character class that matches any alphabetic character.</br>
[abc[:digit:]] is a character class that matches a, b, c, or a digit.</br>
[^[:space:]] is a character class that matches any character except a whitespace.</br>
Escape characters: All escape characters described above can also be used within a character class specification. The only change is with \b, that here is interpreted as a backspace character (\u0008) instead of a word boundary.</br>
Notice that within a class definition, those characters that have a special meaning in the regular expression (such as *, ., $) don't have such a meaning and are interpreted as normal characters (so they do not need to be escaped). Instead, within a class definition, the hyphen (-) and the brackets ([ and ]) do have special meanings under some circumstances, in which case they should be placed within the class in other locations where they do not have such special meaning, or be escaped with a backslash (\).</br>
</br>
Character classes support depend heavily on the regex traits used by the regex object: the regex object calls its traits's isctype member function with the appropriate arguments. For the standard regex_traits object using the default locale, see cctype for a classification of characters.</br>
</br>
Subpatterns (in groups or assertions) can also use the separator operator to separate different alternatives.</br>
</br>

### example 1: strip cpp file comments</br>
//... using \/\/.*$ matches the comment till end</br>
```</br>
abcd//this is comment...</br>
```</br>
</br>
/*anything*/ use \/\*.*\*\/ to match the substring in a line</br>
```</br>
code /*comments!!*/ codes</br>
```</br>
match multiple line comments:</br>
```</br>
code /* comments  </br>
new code ...</br>
abc*/</br>
</br>
code();</br>
//hello</br>
/*start a new function*/</br>
```</br>
The example shall have two matches instead of one.</br>
\/\*(.|[\r\n])*\*\/ this will greedily match from the first /* to the last */ which is incorrect.</br>
</br>
between /* and */ we can have any number of non-space charater and/or any number of blank characters</br>
to disable greedy:</br>
use \/\*(.|[\r\n])*?\*\/ to match 0 or 1 only.</br>
</br>
we can combine the two searches using separator |:</br>
use: \/\/.*$|\/\*(.|[\r\n])*?\*\/</br>
</br>
disable matching in a string:</br>
for example:</br>
```</br>
str="this is a string with /*comment*/ and //others";</br>
```</br>
</br>
example 2:</br>
given the raw:</br>
```</br>
#	Title	Solution	Acceptance	Difficulty	Frequency  </br>
1161	</br>
Maximum Level Sum of a Binary Tree    		71.0%	Medium	</br>
1160	</br>
Find Words That Can Be Formed by Characters    		67.3%	Easy	</br>
1159	</br>
Market Analysis II    		55.0%	Hard	</br>
1158	</br>
Market Analysis I    		63.0%	Medium	</br>
1157	</br>
Online Majority Element In Subarray    		39.3%	Hard	</br>
1156	</br>
Swap For Longest Repeated Character Substring    		47.7%	Medium	</br>
1155	</br>
Number of Dice Rolls With Target Sum    		47.8%	Medium	</br>
1154	</br>
Day of the Year    		49.3%	Easy	</br>
```</br>
we want to put the whole record into one single line.</br>
search for number followed by \r\n and strip the \r\n</br>
(^\d+\s+)\r\n</br>
and replaced with $1:</br>
The output would be:</br>
```</br>
#	Title	Solution	Acceptance	Difficulty	Frequency  </br>
1161	Maximum Level Sum of a Binary Tree    		71.0%	Medium	</br>
1160	Find Words That Can Be Formed by Characters    		67.3%	Easy	</br>
1159	Market Analysis II    		55.0%	Hard	</br>
1158	Market Analysis I    		63.0%	Medium	</br>
1157	Online Majority Element In Subarray    		39.3%	Hard	</br>
1156	Swap For Longest Repeated Character Substring    		47.7%	Medium	</br>
1155	Number of Dice Rolls With Target Sum    		47.8%	Medium	</br>
1154	Day of the Year    		49.3%	Easy	</br>
```</br>
</br>
### using regex in C++</br>
</br>
regex(reg_ex) to build a regex search pattern</br>
can have options:</br>
icase: ignore case</br>
nosubs</br>
optimize</br>
collate</br>
ecmascript</br>
basic</br>
extended</br>
awk</br>
grep</br>
egrep</br>
</br>
regex_search(text,regex_obj): using the regex search pattern to do search in the text.</br>
regex_search return true or false.</br>
it has several forms of overloading functions:</br>
You can get match_results object:</br>
```</br>
match_result res=smatch();</br>
regex_search(text,res,regex_obj)</br>
```</br>
reg_match: check if the whole string matches the pattern. often used for validation of input.</br>
</br>
using slash \:</br>
special meaning char need to be slashed.</br>
</br>
to avold a lot of slashes, we can use c++11 raw string literals</br>
regex(R"((\bmy\b|\byour\b) regex)");</br>
</br>
</br>
More notes:

how regex engine works internally
The Regex-Directed Engine Always Returns the Leftmost Match
This is a very important point to understand: a regex-directed engine will always return the leftmost match, 
even if a "better" match could be found later. When applying a regex to a string, the engine will start at the first character of the string. It will try all possible permutations of the regular expression at the first character. Only if all possibilities have been tried and found to fail, will the engine continue with the second character in the text. Again, it will try all possible permutations of the regex, in exactly the same order. The result is that the regex-directed engine will return the leftmost match.

When applying cat to He captured a catfish for his cat., the engine will try to match the first token in the regex c to the first character in the match H. This fails. There are no other possible permutations of this regex, because it merely consists of a sequence of literal characters. So the regex engine tries to match the c with the e. This fails too, as does matching the c with the space. Arriving at the 4th character in the match, c matches c. The engine will then try to match the second token a to the 5th character, a. This succeeds too. But then, t fails to match p. At that point, the engine knows the regex cannot be matched starting at the 4th character in the match. So it will continue with the 5th: a. Again, c fails to match here and the engine carries on. At the 15th character in the match, c again matches c. The engine then proceeds to attempt to match the remainder of the regex at character 15 and finds that a matches a and t matches t.

The entire regular expression could be matched starting at character 15. The engine is "eager" to report a match. It will therefore report the first three letters of catfish as a valid match. The engine never proceeds beyond this point to see if there are any "better" matches. The first match is considered good enough.

In this first example of the engine's internals, our regex engine simply appears to work like a regular text search routine. A text-directed engine would have returned the same result too. However, it is important that you can follow the steps the engine takes in your mind. In following examples, the way the engine works will have a profound impact on the matches it will find. Some of the results may be surprising. But they are always logical and predetermined, once you know how the engine works.

negated character class: [^u] it also matches \r\n (unlike .)
inside a character class, the special character allowed:
[]^\
\s matches space, tab, \r, \n, \s=[ \t\r\n]

inside:
As I already said: the order of the characters inside a character class does not matter. gr[ae]y will match grey in Is his hair grey or gray?, because that is the leftmost match. We already saw how the engine applies a regex consisting only of literal characters. Below, I will explain how it applies a regex that has more than one permutation. That is: gr[ae]y can match both gray and grey.

Nothing noteworthy happens for the first twelve characters in the string. The engine will fail to match g at every step, and continue with the next character in the string. When the engine arrives at the 13th character, g is matched. The engine will then try to match the remainder of the regex with the text. The next token in the regex is the literal r, which matches the next character in the text. So the third token, [ae] is attempted at the next character in the text (e). The character class gives the engine two options: match a or match e. It will first attempt to match a, and fail.

But because we are using a regex-directed engine, it must continue trying to match all the other permutations of the regex pattern before deciding that the regex cannot be matched with the text starting at character 13. So it will continue with the other option, and find that e matches e. The last regex token is y, which can be matched with the following character as well. The engine has found a complete match with the text starting at character 13. It will return grey as the match result, and look no further. Again, the leftmost match was returned, even though we put the a first in the character class, and gray could have been matched in the string. But the engine simply did not get that far, because another equally valid match was found to the left of it.

.=[^\r\n]
activating multiline mode will make . match \n.

anchors:
integer input:
^\s*\d+\s*$: leading and tailing spaces are allowed.
13
  25 
749
486
4
\d* will make zero length match a success.

\b: word boundary, match not a word char.
a word character [0-9a-zA-Z_] or \w.
\B: negate \b.

If you want to search for the literal text cat or dog, separate both options with a vertical bar or pipe symbol: cat|dog. If you want more options, simply expand the list: cat|dog|mouse|fish .

The alternation operator has the lowest precedence of all regex operators. That is, it tells the regex engine to match either everything to the left of the vertical bar, or everything to the right of the vertical bar. If you want to limit the reach of the alternation, you will need to use round brackets for grouping. If we want to improve the first example to match whole words only, we would need to use \b(cat|dog)\b. This tells the regex engine to find a word boundary, then either "cat" or "dog", and then another word boundary. If we had omitted the round brackets, the regex engine would have searched for "a word boundary followed by cat", or, "dog followed by a word boundary.

? makes eh preceding token optional
colou?r: will match color or colour.
? 0 or 1 matches.
?? turn off greedy (lazy)

With the question mark, I have introduced the first metacharacter that is greedy. The question mark gives the regex engine two choices: try to match the part the question mark applies to, or do not try to match it. The engine will always try to match that part. Only if this causes the entire regular expression to fail, will the engine try ignoring the part the question mark applies to.

The effect is that if you apply the regex Feb 23(rd)? to the string Today is Feb 23rd, 2003, the match will always be Feb 23rd and not Feb 23. You can make the question mark lazy (i.e. turn off the greediness) by putting a second question mark after the first.










