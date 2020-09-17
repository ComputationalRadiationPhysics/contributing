# CRP & CASUS C++ Guidelines

## General considerations

### Lines are restricted to 79 columns.

**Reason:** While 80 columns are a good value to view code side by side this
does not take `diff` into account which adds another character.

**Enforced by:**
  * `.clang-format`: `ColumnLimit: 79`
  * `.clang-format`: `BreakStringLiterals: true`
  * `.clang-format`: `PenaltyBreakAssignment: 2`
  * `.clang-format`: `PenaltyBreakBeforeFirstCallParameter: 19`
  * `.clang-format`: `PenaltyBreakComment: 300`
  * `.clang-format`: `PenaltyBreakFirstLessLess: 120`
  * `.clang-format`: `PenaltyBreakString: 1000`
  * `.clang-format`: `PenaltyBreakTemplateDeclaration: 10`
  * `.clang-format`: `PenaltyExcessCharacter: 1000000`
  * `.clang-format`: `PenaltyReturnTypeOnItsOwnLine: 1000`
  * `.clang-format`: `ReflowComments: true`

### Use UNIX-style linebreaks.

**Reason:** Some editors interpret CRLF (`\r\n`) as two linebreaks. LF (`\n`)
is always interpreted correctly.

**Enforced by:**
  * `.clang-format`: `DeriveLineEnding: false`
  * `.clang-format`: `UseCRLF: false`

## Code structure

### Do not place short blocks on single lines.

**Reason:** This easily leads to bugs.

**Exception:** Empty lambda functions.

**Enforced by:**
  * `.clang-format`: `AllowShortBlocksOnASingleLine: Never`
  * `.clang-format`: `AllowShortCaseLabelsOnASingleLine: false`
  * `.clang-format`: `AllowShortEnumsOnASingleLine: false`
  * `.clang-format`: `AllowShortFunctionsOnASingleLine: None`
  * `.clang-format`: `AllowShortIfStatementsOnASingleLine: Never`
  * `.clang-format`: `AllowShortLambdasOnASingleLine: Empty`
  * `.clang-format`: `AllowShortLoopsOnASingleLine: false`

**Example:**
```c++
if(foo) return; // bad

if(bar)         // good
    return;
```

### Use whitespace to visually structure the code where it makes sense.

**Reason:** C++ can be hard to read at times. There is no reason to
unnecessarily increase the visual density.

**Enforced by:**
  * `.clang-format`: `AlwaysBreakTemplateDeclarations: Yes`
  * `.clang-format`: `BinPackArguments: false`
  * `.clang-format`: `BinPackParameters: false`
  * `.clang-format`: `SpaceAfterCStyleCast: true`
  * `.clang-format`: `SpaceAfterTemplateKeyword: true`
  * `.clang-format`: `SpaceBeforeAssignmentOperators: true`
  * `.clang-format`: `SpaceBeforeCpp11BracedList: true`
  * `.clang-format`: `SpaceBeforeCtorInitializerColon: true`
  * `.clang-format`: `SpaceBeforeInheritanceColon: true`
  * `.clang-format`: `SpaceBeforeParens: ControlStatements`
  * `.clang-format`: `SpaceBeforeRangeBasedForLoopColon: true`

**Example:**
```c++
for(auto x: container) // bad
{
    /* ... */
}

for(auto x : container) // good
{
    /* ... */
}
```

### Do not overuse whitespace.

**Reason:** C++ already needs a lot of screen space. There is no reason to
unnecessarily bloat the screen any further.

**Enforced by:**
  * `.clang-format`: `AlwaysBreakAfterReturnType: None`
  * `.clang-format`: `AlwaysBreakBeforeMultilineStrings: false`
  * `.clang-format`: `Cpp11BracedListStyle: true`
  * `.clang-format`: `IndentWrappedFunctionNames: false`
  * `.clang-format`: `KeepEmptyLinesAtTheStartOfBlocks: false`
  * `.clang-format`: `MaxEmptyLinesToKeep: 2`
  * `.clang-format`: `SpaceAfterLogicalNot: false`
  * `.clang-format`: `SpaceBeforeSquareBrackets: false`
  * `.clang-format`: `SpaceInEmptyBlock: false`
  * `.clang-format`: `SpaceInEmptyParentheses: false`
  * `.clang-format`: `SpacesBeforeTrailingComments: 1`
  * `.clang-format`: `SpacesInAngles: false`
  * `.clang-format`: `SpacesInConditionalStatement: false`
  * `.clang-format`: `SpacesInContainerLiterals: false`
  * `.clang-format`: `SpacesInCStyleCastParentheses: false`
  * `.clang-format`: `SpacesInParentheses: false`
  * `.clang-format`: `SpacesInSquareBrackets: false`

**Example:**
```c++
int                          // There is no reason for a line break
foo();

int bar( float x, float y ); // There is no reason for the spaces here
float array[ 10 ];           // Or here
baz< double >(...);          // Or here
```

### Use namespace end comments.

**Reason:** Namespaces can span many lines. It is probably not obvious to
which namespace a closing `}` belongs.

**Enforced by:**
  * `.clang-format`: `FixNamespaceComments: true`

**Example:**
```c++
namespace bad_example
{
}

namespace good_example
{
} // good_example
```

### Put nested namespaces on separate lines.

If available the C++17 syntax is preferred: `namespace first::second`.

**Reason**: Each statement belongs on its own line. This includes namespace
declarations.

**Enforced by:**
  * `.clang-format`: `CompactNamespaces: false`

**Example:**
```c++
// bad example
namespace foo { namespace bar {
}}

// good example
namespace foo
{
    namespace bar
    {
    }
}

// best example
namespace foo::bar
-{
}
```

### Put constructor initializers and inheritance list members on separate lines.

**Reason:** It is easier to read multiple entries one-by-one than having them
arbitrarily spread across lines.

**Exception**: All initializers / list members fit into one line.

**Enforced by:**
  * `.clang-format`: `ConstructorInitializerAllOnOneLineOrOnePerLine: true`

**Example:**
```c++
class foo
{
    foo()
        : aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa{val}
        , bbbbbbbbbbbbb{another}
        , ccccccccccccccccccccccccccc{value}
    {}
};
```

## Indentation and alignment.

### Use the Allman indentation style.

**Reason:** This gives the code a nice vertical structure and optically
separates different scopes from another. The K&R indentation style
unnecessarily increases code density, making it harder to follow its
structure.

**Enforced by:**
  * `.clang-format`: `BreakBeforeBraces: Allman`

**Example:**
```c++
// bad example
if(foo) {
    /* something */
}

// good example
if(bar)
{
    /* something */
}
```

### Use spaces, never tabs.

**Reason:** Spaces are the same on all machines, tabs are not.

**Enforced by:**
  * `.clang-format`: `UseTab: Never`

### The indentation width is set to four columns.

**Reason**: 80 (the maximum line width) is a multiple of four. Two columns do 
not provide enough whitespace for visual structuring, eight columns (and
above) fill the 80 columns too fast.

**Enforced by:**
  * `.clang-format`: `IndentWidth: 4`
  * `.clang-format`: `TabWidth: 4`

### Align after open brackets.

**Reason:** This aligns multiple parameters nicely that do not fit into a
single line. This rule applies to `()`, `<>` and `[]`.

**Enforced by:**
  * `.clang-format`: `AlignAfterOpenBracket: Align`
  * `.clang-format`: `AllowAllArgumentsOnNextLine: false`
  * `.clang-format`: `AllowAllConstructorInitializersOnNextLine: false`
  * `.clang-format`: `AllowAllParametersOfDeclarationOnNextLine: false`

**Example:**
```c++
auto some_long_function(argument1,
                        argument2);
```

### Align consecutive assignments, bitfields and macros.

**Reason:** It is visually pleasing and there is no reason against it.

**Enforced by:**:
  * `.clang-format`: `AlignConsecutiveAssignments: true`
  * `.clang-format`: `AlignConsecutiveBitFields: true`
  * `.clang-format`: `AlignConsecutiveDeclarations: false`
  * `.clang-format`: `AlignEscapedNewlines: Right`
  * `.clang-format`: `BitFieldColonSpacing: Both`

**Example:**
```c++
float abcdef = 42.f;
int xyz      = 3;
double klmn  = 10.0;

int aaaaaaa : 42;
int bbbb    : 21;
int cc      : 10;

#define SHORT                  10
#define VERY_VERY_LONG         21
#define THIS_IS_ABSURDELY_LONG 42
#define evil_func(x)           (x + x)
#define another_evil_func(x)   (x * x)

#define FOO                             \
    int aaaaaa;                         \
    int bbbb;                           \
    int cc;                             \
```

### Align operators, colons and commas on the left hand side.

**Reason:** For operators this follows the syntax you would use by writing by
hand. The commas and colons are kept here for consistency.

**Enforced by:**
  * `.clang-format`: `AlignOperands: true`
  * `.clang-format`: `BreakBeforeBinaryOperators: All`
  * `.clang-format`: `BreakBeforeTernaryOperators: true`
  * `.clang-format`: `BreakConstructorInitializers: BeforeComma`
  * `.clang-format`: `BreakInheritanceList: BeforeComma`
  * `.clang-format`: `OperandAlignmentStyle: Align`

**Example:**
```c++
int aaa = bbbbbbbbbb
        + cccccccccc;

class foo
    : public first_parent
    , public second_parent
{
    foo()
        : first_member{some_value}
        , second_member{another_value}
};
```

### Align trailing comments.

**Reason:** This makes the documentation easier to read.

**Enforced by:**
  * `.clang-format`: `AlignTrailingComments: true`

**Example:**
```c++
int aaaaaaaaa; // A comment
int bbb;       // Another comment.
```

### Continued lines are indented.

**Reason:** This helps to visually separate them from the surrounding scope.

**Enforced by:**
  * `.clang-format`: `ContinuationIndentWidth: 4`

**Example:**
```c++
auto foo = some_long_initializer{value1,
    value2, {inner_value_1,
        inner_value_2}};
```

### Case labels are not indented.

**Reason**: Case labels do not open a new scope. Indentation helps to
differentiate between scopes.

**Enforced by:**
  * `.clang-format`: `IndentCaseLabels: false`

**Example:**
```c++
switch(statement)
{
case first_case:
    /* something */
    break;

case second_case:
    /* something else */
    break;
}
```

### Case blocks are indented.

**Reason**: Case blocks are part of a scope (opened by `switch`) and should be
indented accordingly.

**Enforced by:**
  * `.clang-format`: `IndentCaseBlocks: true`

```c++
switch(statement)
{
case first_case:
    /* something */
    break;

case second_case:
    /* something else */
    break;
}
```

### `extern "C"` blocks are indented.

**Reason:** `extern "C"` opens a new scope (with special semantics). It
should be indented accordingly.

**Enforced by:**
  * `.clang-format`: `IndentExternBlock: AfterExternBlock`

### `goto` labels are not indented.

***DO NOT USE `goto`!***

**Reason:** goto labels do not open scopes. There is no reason to add an
extra level of indentation to the following lines.

**Enforced by:**
  * `.clang-format`: `IndentGotoLabels: false`

### Nested preprocessor directives are indented without the hash.

**Reason:** This helps to easily identify lines that target the preprocessor.

**Enforced by:**
  * `.clang-format`: `IndentPPDirectives: AfterHash`

**Example:**
```c++
#if defined(something)
#   if defined(something_else)
#       define FOO BAR
#   else
#       define FOO BAZ
#   endif
#endif
```

### Inheritance lists and constructor initializers are indented.
**Reason:** They are part of the class / constructor scope and thus are
indented.

**Enforced by:**
  * `.clang-format`: `ConstructorInitializerIndentWidth: 4`

### Class access modifiers are not indented.

**Reason:** Indention marks a scope. Class access modifiers do not open
separate scopes. This also follows the style of the C++ standard and is the
default setting of many IDEs.

**Enforced by:**
  * `.clang-format`: `AccessModifierOffset: -4`

**Example:**
```c++
class foo
{
public:
    /* The interface */
private:
    /* The secrets. */
};
```

### Namespace members and nested namespaces are indented.

**Reason:** This follows the overall code style.

**Enforced by:**
  * `.clang-format`: `NamespaceIndentation: All`

## Code style

### Align pointers and references to their type.

**Reason:** Pointers and references are part of the type and should be
part of the declaration as well.

**Enforced by:**
  * `.clang-format`: `DerivePointerAlignment: false`
  * `.clang-format`: `PointerAlignment: left`

**Example:**
```c++
int *bad;
int * worse;
int* good;
```

## Undocumented settings

* `.clang-format`: `Language: Cpp`
* `.clang-format`: `Standard: c++17`
* `.clang-format`: `DisableFormat: false`
* `.clang-format`: `ExperimentalAutoDetectBinPacking: false`
* `.clang-format`: `IncludeBlocks: Preserve`
* `.clang-format`: `IncludeCategories:`
```yaml
  - Regex:           '^"(llvm|llvm-c|clang|clang-c)/'
    Priority:        2
    SortPriority:    0
  - Regex:           '^(<|"(gtest|gmock|isl|json)/)'
    Priority:        3
    SortPriority:    0
  - Regex:           '.*'
    Priority:        1
    SortPriority:    0
```
* `.clang-format`: `IncludeIsMainRegex: '(Test)?$'`
* `.clang-format`: `IncludeIsMainSourceRegex: ''`
* `.clang-format`: `MacroBlockBegin: ''`
* `.clang-format`: `MacroBlockEnd: ''`
* `.clang-format`: `SortIncludes: true`
* `.clang-format`: `SortUsingDeclarations: true`
