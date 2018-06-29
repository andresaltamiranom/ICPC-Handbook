# Shunting Yard

The Shunting Yard algorithm is a method for parsing mathematical expressions specified in infix notation to produce a postfix notation string, also known as _Reverse Polish Notation_. This algorithm is **stack-based**. Infix expressions are the form of mathematical notation most people are used to, like "3 + 4" or "3 + 4 × (2 − 1)". For the conversion, there are two string variables: the input and the output. There is also a stack that holds operators not yet added to the output queue. To convert, the program reads each symbol in order and does something based on that symbol. The result for the above examples would be "3 4 +" or "3 4 2 1 − × +".

Following is an illustration of this problem:

![Shunting Yard](https://upload.wikimedia.org/wikipedia/commons/2/24/Shunting_yard.svg)

The following code solves this problem, receiving the reference to an input stream and to an output stream of characters as parameters. The former contains the expression in infix notation and the latter will hold the postfix notation when the process completes.

```cpp

void output(ostream &out, string x) {
    out << x << " ";
}

string readToken(istream &in) {
    string t; int c;
    while((c = in.peek()) != EOF) {
        if(isalpha(c) || isdigit(c)) t.pb((char)c), in.get();
        else if(t != "") return t;
        else {in.get(); if(!isspace(c)) {t.pb((char)c); return t;}}
    } return t;
}

#define LEFT 0
#define RIGHT 1
#define isOp(x) (prec.find(x) != prec.end())
void shunting(istream &in, ostream &out) {
    string token;
    stack<string> ops;
    map<string, int> prec;
    prec["^"] = 6;
    prec["*"] = prec["/"] = prec["%"] = 5;
    prec["+"] = prec["-"] = 4;
    map<string, int> assoc; // default 0
    assoc["^"] = RIGHT;
    while((token = readToken(in)) != "") {
        if(isOp(token)) {
            while(!ops.empty() && isOp(ops.top())
            && ((assoc[token] == LEFT && prec[token] <= prec[ops.top()]) 
                 || (assoc[token] == RIGHT && prec[token] < prec[ops.top()])))
                output(out, ops.top()), ops.pop();
            ops.push(token);
        } else if(token == "(") {
            ops.push(token);
        } else if(token == ")") {
            while(!ops.empty() && ops.top() != "(")
                output(out, ops.top()), ops.pop();
            // ops.empty() || ops.top() != "(" ====> MISMATCH
            ops.pop();
        } else // numbers vars
            output(out, token);
    }
    while(!ops.empty()) { // if ops.top() == ")"  || ops.top() == "(" =======> MISMATCH
        output(out, ops.top()), ops.pop(); 
    }
}

```
