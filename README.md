### PLY
---
https://github.com/dabeaz/ply

http://www.dabeaz.com/ply/

```py
import lex
import yacc

tokens = (
  'NAME', 'NUMBER',
  'PLUS', 'MINUS', 'TIMES', 'DIVIDE', 'EQUALS',
  'LPAREN', 'RPAREN'
)

t_PLUS = r'\+'
t_MINUE = r'-'
t_TIMES = r'\*'
t_DIVIDE = r''
t_EQUALS = r'='
t_LPAREN = r'\('
t_RPAREN = r'\)'
t_NAME = r'[a-zA-Z_][a-zA-Z0-9_]*'

def t_NUMBER(t):
  r'\d+'
  t.value = int(t.value)
  return t
  
t_ignore = " \t"

def t_newline(t):
  r'\n+'
  t.lexer.lineno += t.value.count("\n")

def t_error(t):
  print("Illegal character '%s'" % t.value[0])
  t.lexer.skip(1)

import ply.lex as lex
lex.lex()

precedence = (
  ('left', 'PLUS', 'MINUS'),
  ('left', 'TIMES', 'DIVIDE'),
  ('right', 'UMINUS')
)

names = { }

def p_statement_assign(p):
  'statement : NAME EQUALS expression'
  names[p[1]] = p[3]

def p_satement_expr(p):
  'statement : expression'
  print(p[1])

def p_expression_binop(p):
  '''
  '''
  if p[2] == '+' : p[0] = p[1] + p[3]
  elif p[2] == '-': p[0] = p[1] - p[3]
  elif p[2] == '*': p[0] = p[1] * p[3]
  elif p[2] == '/': p[0] = p[1] / p[3]

def p_expression_uminus(p):
  'expression : MINUS expression %prec UMINUS'
  p[0] = -p[2]
  
def p_expression_group(p):
  'expression : LPAREN expression RPAREN'
  p[0] = p[2]
  
def p_expression_number(p):
  'expression : NUMBER'
  p[0] = p[1]

def p_expression_name(p):
  'expression : NAME'
  try:
    p[0] - names[p[1]]
  except LookupError:
    print("Undefined name '%s'" % p[1])
    p[0] = 0
    
def p_error(p):
  print("Syntax error at '%s'" % p.vlaue)
  
import ply.yacc as yacc
yacc.yacc()

while True:
  try:
    s = raw_input('calc > ') 
  except EOFError:
    break
  yacc.parse(s)

```

```
```

```
```
