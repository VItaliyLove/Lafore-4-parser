# Lafore-4-parser

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.nio.Buffer;

class Algorithm {

    static class Stack {

        private char[] arr;
        private int top;

        public Stack(int max) {
            arr = new char[max];
            top = -1;
        }

        public void push(char ch) {
            top++;
            arr[top] = ch;
        }

        public char pop() {
            return arr[top--];
        }

        public void display() {
            for (char x : arr) {
                System.out.print(x);
            }
            System.out.println();
        }

        public boolean isEmpty() {
            return (top == -1);
        }

        public int getSize() {
            return top+1;
        }
    }

    static class IntStack {

        private int[] arr;
        private int top;

        public IntStack(int max) {
            arr = new int[max];
            top = -1;
        }

        public void push(int num) {
            top++;
            arr[top] = num;
        }

        public int pop() {
            return arr[top--];
        }

        public void display() {
            for (int x : arr) {
                System.out.print(x);
            }
            System.out.println();
        }

        public boolean isEmpty() {
            return (top == -1);
        }

    }

    static class InToPos {

        private String input;
        private String output;
        Stack stack;

        public InToPos(String in) {

            input = in;
            output = "";
            stack = new Stack(input.length());

        }

        private void gotOper(char ch, int prior1) {

            while (!stack.isEmpty()) {
                char top = stack.pop();
                if (top == '(') {
                    stack.push(top);
                    break;
                }
                else {
                    int prior2;
                    if (top == '-' || top == '+')
                        prior2 = 1;
                    else
                        prior2 = 2;
                    if (prior2 < prior1) {
                        stack.push(top);
                        break;
                    }
                    else
                        output += top;
                }
            }
            stack.push(ch);

        }

        private void gotParen() {
            while (!stack.isEmpty()) {
                char top = stack.pop();
                if (top == '(')
                    break;
                else
                    output += top;
            }
        }

        public String doTrans() {
            for (int i = 0; i < input.length(); i++) {
                char ch = input.charAt(i);
                switch (ch) {
                    case '-':
                    case '+':
                        gotOper(ch,1);
                        break;
                    case '/':
                    case '*':
                        gotOper(ch,2);
                        break;
                    case '(':
                        stack.push(ch);
                        break;
                    case ')':
                        gotParen();
                        break;
                    default:
                        output += ch;
                }
            }
            while (!stack.isEmpty()) {
                output += stack.pop();
            }
            return output;
        }
    }

    static class Parser {

        private IntStack stack;
        private String str;

        public Parser(String s) {
            str = s;
            stack = new IntStack(str.length());
        }

        public int parse() {
            int num1, num2, i, answer;
            char ch;
            for (i = 0; i < str.length(); i++) {
                ch = str.charAt(i);
                if (ch >= '0' && ch <= '9')
                    stack.push((int)ch-'0');
                else {
                    num1 = stack.pop();
                    num2 = stack.pop();
                    switch (ch) {
                        case '-':
                            answer = num1-num2;
                            break;
                        case '+':
                            answer = num1+num2;
                            break;
                        case '*':
                            answer = num1*num2;
                            break;
                        case '/':
                            answer = num1/num2;
                            break;
                        default:
                            answer = 0;
                    }
                    stack.push(answer);
                }
            }
            return (stack.pop());
        }
    }

    public static void main(String[] arg) throws IOException {

        String myS = "  2  *  3  +  4  *  5  +  6  *  7  ";
        InToPos inToPos;
        Parser parser;
        int result;
        inToPos = new InToPos(myS);
        myS = inToPos.doTrans();
        myS = myS.replaceAll(" ","");
        System.out.println(myS);
        parser = new Parser(myS);
        result = parser.parse();
        System.out.println(result);

    }
}
