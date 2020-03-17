# sql-formatter
Configurable SQL formatter


# Instalation

```bash
npm install @simonwardjones/sql-formatter
```

# Usage

```javascript
var sql = require('@simonwardjones/sql-formatter');
tokenizer = new sql.Tokenizer()
console.log(tokenizer.tokenize('select * from schema.table'))
```

# Publish
> note this install calls prepare from scripts

```bash
npm publish --access public .
```

# To DO
- [X] Add to npm `@simonwardjones/sql-formatter`
- [X] ~Use function of contexts for context depth~ N/A context depth not equal to contextStack lenght (inline contexts)
- [X] Check word breaks after regexp e.g. `selectselect` (added test case)
- [X] Get rid or sql block type - not requred any more!
- [X] Fix when for case statement!
- [X] Handle in key word levels in different functions
- [X] Fix or/and in BLOCK
- [X] When error write the first half formatted then write out raw tokens
- [X] Don't lower comments or strings
- [X] Move all examples from demo to test cases
- [X] ~Use contexts from config~ Can't think why I said that
- [X] Calculate the exact line length
- [X] Move functions to config for open parenthesis layout
- [X] improve numeric
- [X] add dot symbol (then identifiers are words/string identifiers before or after dots or after from/join/table)
- [X] test and fix set operators impact on select depth (should - 1)
- [ ] change identifier to just string identifiers and fix tests
- [ ] Enable capitalisation of alias
- [ ] Implement key word cases
- [ ] parametrise some things and, on, nl after from or select
- [ ] Update comments and README with detail on logic and rules
- [ ] fix bug with expression after comment in select blovk

# Layout Rules

### general

Considerations:
 - Expressions should be indented (assuming we are in a query)
 - Key words like from shouldn't be

Rules:
    - top level words start new line at current depth e.g. select from
        - some of these demand a newline after as well, e.g. select
    - commas start new line with current depth + indent
    - commas should act differenetly in row number for example
    - tokens should write space before unless they are the first word.

### Parenthesis

Considerations:
 - mathematical grouping
 - sub queries 
 - functions e.g. count(*)

Rules:
 - write the open in the current context then push tokens to stack checking how long it is until close
    - if we meet a reserved word start writing the tokens as normal


### Commas 
Considerations:
 - order by numbers shouldn't start new row, words should.
 - partition by shouldn't start new row unless really long

### Reserved Words

Rules
 - If first newLineReservedWord just write


### Tokenizer Logic

 - The tokenizer has tokenTypes (a list of TokenTypes) and an empty token list
 - The method Tokenizer.tokenize takes in a string and then repeatedly calls the Tokenizer.eatToken until all the string is tokenized or no token can be found
    - Tokenizer.eatToken loops through the tokenTypes in order trying to match patterns by calling TokenType.eatToken, if a token is found the Tokenizer.eatToken method appends the token to the Tokenizer.tokens and returns the rest of the string
    - TokenType.eatToken just returns the string and a token if there is one
 - Tokenizer.tokenize finally returns the list of tokens
 - If an error takes place i.e. no token matches the error token is returned (the rest of the string)