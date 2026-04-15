#include <stdio.h>
#include <math.h>
#include <stdlib.h>
#include <string.h>

double Ans = 0;   // Global storage for the last calculated result

double get_value(const char* prompt) {
    char input[50];
    printf("%s (type 'ans' to use %.4lf): ", prompt, Ans);
    scanf("%s", input);
    
    if (strcasecmp(input, "ans") == 0) {
        return Ans;
    }
    return atof(input);
}

int get_int_value(const char* prompt) {
    char input[50];
    printf("%s (type 'ans' to use %d): ", prompt, (int)Ans);
    scanf("%s", input);
    
    if (strcasecmp(input, "ans") == 0) {
        return (int)Ans;
    }
    return atoi(input);
}

void matrix_add() {
    int r, c;
    printf("Enter rows and cols: ");
    scanf("%d %d", &r, &c);

    double A[10][10], B[10][10];

    printf("\nEnter Matrix A elements:\n");
    for (int i = 0; i < r; i++) {
        for (int j = 0; j < c; j++) {
            printf("A[%d][%d]: ", i + 1, j + 1);
            scanf("%lf", &A[i][j]);
        }
    }

    printf("\nEnter Matrix B elements:\n");
    for (int i = 0; i < r; i++) {
        for (int j = 0; j < c; j++) {
            printf("B[%d][%d]: ", i + 1, j + 1);
            scanf("%lf", &B[i][j]);
        }
    }

    printf("\nMatrix Addition Result (Sum):\n");
    for (int i = 0; i < r; i++) {
        for (int j = 0; j < c; j++) {
            double sum = A[i][j] + B[i][j];
            printf("%.2lf ", sum);
            Ans = sum; // Stores the very last calculated cell in Ans
        }
        printf("\n");
    }
}

void matrix_multiply() {
    int r1, c1, r2, c2;
    printf("Enter rows & cols of Matrix A: ");
    scanf("%d %d", &r1, &c1);
    printf("Enter rows & cols of Matrix B: ");
    scanf("%d %d", &r2, &c2);

    if (c1 != r2) {
        printf("\nInvalid! Columns of A must equal rows of B for multiplication.\n");
        return;
    }

    double A[10][10], B[10][10], C[10][10];

    printf("\nEnter elements for Matrix A:\n");
    for (int i = 0; i < r1; i++) {
        for (int j = 0; j < c1; j++) {
            printf("A[%d][%d]: ", i + 1, j + 1);
            scanf("%lf", &A[i][j]);
        }
    }

    printf("\nEnter elements for Matrix B:\n");
    for (int i = 0; i < r2; i++) {
        for (int j = 0; j < c2; j++) {
            printf("B[%d][%d]: ", i + 1, j + 1);
            scanf("%lf", &B[i][j]);
        }
    }

    for (int i = 0; i < r1; i++) {
        for (int j = 0; j < c2; j++) {
            C[i][j] = 0;
            for (int k = 0; k < c1; k++)
                C[i][j] += A[i][k] * B[k][j];
        }
    }

    printf("\nMatrix Multiplication Result (Product):\n");
    for (int i = 0; i < r1; i++) {
        for (int j = 0; j < c2; j++) {
            printf("%.2lf ", C[i][j]);
            Ans = C[i][j];
        }
        printf("\n");
    }
}

void inverse2x2() {
    double a, b, c, d;
    printf("Enter elements of 2x2 matrix:\n");
    
    printf("Element [1][1]: "); scanf("%lf", &a);
    printf("Element [1][2]: "); scanf("%lf", &b);
    printf("Element [2][1]: "); scanf("%lf", &c);
    printf("Element [2][2]: "); scanf("%lf", &d);

    double det = a * d - b * c;

    if (det == 0) {
        printf("\nMatrix is singular! Inverse not possible.\n");
        return;
    }

    printf("\nInverse matrix results:\n");
    printf("%.4lf %.4lf\n",  d / det, -b / det);
    printf("%.4lf %.4lf\n", -c / det,  a / det);
    
    Ans = det; 
}

void quadratic() {
    double a, b, c;
    printf("Equation format: ax^2 + bx + c = 0\n");
    a = get_value("Enter a");
    b = get_value("Enter b");
    c = get_value("Enter c");

    if (a == 0) {
        printf("\nNot a quadratic equation (a cannot be 0).\n");
        return;
    }

    double D = b * b - 4 * a * c;

    if (D > 0) {
        double r1 = (-b + sqrt(D)) / (2 * a);
        double r2 = (-b - sqrt(D)) / (2 * a);
        printf("\nReal and distinct roots found:\n");
        printf("Root 1: %.4lf\n", r1);
        printf("Root 2: %.4lf\n", r2);
        Ans = r1;
    } 
    else if (D == 0) {
        double r = -b / (2 * a);
        printf("\nReal and equal roots found:\n");
        printf("Root: %.4lf\n", r);
        Ans = r;
    } 
    else {
        double real = -b / (2 * a);
        double imag = sqrt(-D) / (2 * a);
        printf("\nComplex roots found:\n");
        printf("%.4lf + %.4lfi\n", real, imag);
        printf("%.4lf - %.4lfi\n", real, imag);
        Ans = real;
    }
}

double cubic_f(double a, double b, double c, double d, double x) {
    return a * x * x * x + b * x * x + c * x + d;
}

double cubic_df(double a, double b, double c, double x) {
    return 3 * a * x * x + 2 * b * x + c;
}

void cubic() {
    double a, b, c, d;
    printf("Equation: ax^3 + bx^2 + cx + d = 0\n");
    a = get_value("Enter a");
    b = get_value("Enter b");
    c = get_value("Enter c");
    d = get_value("Enter d");

    double x = 1.0; 
    for (int i = 0; i < 10000; i++) {
        double fx = cubic_f(a, b, c, d, x);
        double fdx = cubic_df(a, b, c, x);
        if (fdx == 0) break; 
        x = x - fx / fdx;
    }

    printf("\nNumerical Analysis Result:\n");
    printf("One real root (approx) = %.6lf\n", x);
    printf("Remaining roots may be complex or require further analysis.\n");
    Ans = x;
}

long long factorial(int n) {
    if (n < 0) return 0;
    long long fact = 1;
    for (int i = 1; i <= n; i++)
        fact *= i;
    return fact;
}

int main() {
    int choice;
    char buffer[100];

    while (1) {
        printf("\n=================================================\n");
        printf("            CASIO SCIENTIFIC CALCULATOR          \n");
        printf("=================================================\n");
        printf(" Current Memory (Ans) = %.4lf\n", Ans);
        printf("-------------------------------------------------\n");
        printf(" 1.  Addition (Summation)\n");
        printf(" 2.  Subtraction\n");
        printf(" 3.  Multiplication\n");
        printf(" 4.  Division\n");
        printf(" 5.  Power Chain (x^y^z...)\n");
        printf(" 6.  Square Root\n");
        printf(" 7.  Factorial (!)\n");
        printf(" 8.  Logarithm (base 10)\n");
        printf(" 9.  Natural Logarithm (ln)\n");
        printf(" 10. Trigonometry: Sin\n");
        printf(" 11. Trigonometry: Cos\n");
        printf(" 12. Trigonometry: Tan\n");
        printf(" 13. Permutations (nPr)\n");
        printf(" 14. Combinations (nCr)\n");
        printf(" 15. Statistical Mean\n");
        printf(" 16. Find Maximum\n");
        printf(" 17. Find Minimum\n");
        printf(" 18. Matrix Addition\n");
        printf(" 19. Matrix Multiplication\n");
        printf(" 20. Matrix Inverse (2x2)\n");
        printf(" 21. Quadratic Solver\n");
        printf(" 22. Cubic Solver (Real Root)\n");
        printf(" 23. View Last Answer\n");
        printf(" 0.  Exit System\n");
        printf("-------------------------------------------------\n");
        printf("Select an operation: ");
        
        if (scanf("%d", &choice) != 1) {
            printf("\nInvalid input type detected. Restarting menu...\n");
            while(getchar() != '\n'); 
            continue;
        }

        if (choice == 0) {
            printf("\nShutting down calculator. Goodbye!\n");
            break;
        }

        int n;
        double num, result;

        switch (choice) {
            case 23:
                printf("\nThe last calculated result in memory is: %.4lf\n", Ans);
                break;

            case 1:
                printf("How many numbers to add? ");
                scanf("%d", &n);
                result = 0;
                for (int i = 1; i <= n; i++) {
                    sprintf(buffer, "Number %d", i);
                    result += get_value(buffer);
                }
                Ans = result;
                printf("\nTotal Sum = %.4lf\n", result);
                break;

            case 2:
                printf("How many numbers in the sequence? ");
                scanf("%d", &n);
                result = get_value("Starting number");
                for (int i = 2; i <= n; i++) {
                    sprintf(buffer, "Subtract number %d", i);
                    result -= get_value(buffer);
                }
                Ans = result;
                printf("\nFinal Difference = %.4lf\n", result);
                break;

            case 3:
                printf("How many numbers to multiply? ");
                scanf("%d", &n);
                result = 1;
                for (int i = 1; i <= n; i++) {
                    sprintf(buffer, "Factor %d", i);
                    result *= get_value(buffer);
                }
                Ans = result;
                printf("\nProduct = %.4lf\n", result);
                break;

            case 4:
                printf("How many numbers in the division chain? ");
                scanf("%d", &n);
                result = get_value("Dividend");
                for (int i = 2; i <= n; i++) {
                    sprintf(buffer, "Divisor %d", i);
                    num = get_value(buffer);
                    if (num == 0) {
                        printf("\nMath Error! Division by zero is undefined.\n");
                        goto next_loop;
                    }
                    result /= num;
                }
                Ans = result;
                printf("\nQuotient = %.4lf\n", result);
                break;

            case 5: {
                printf("How many stages in the power chain? ");
                scanf("%d", &n);
                double arr[20];
                for (int i = 0; i < n; i++) {
                    sprintf(buffer, "Base/Exp %d", i + 1);
                    arr[i] = get_value(buffer);
                }
                double p = arr[0];
                for (int i = 1; i < n; i++)
                    p = pow(p, arr[i]);
                Ans = p;
                printf("\nChain Result = %.4lf\n", p);
                break;
            }

            case 6:
                num = get_value("Enter number for square root");
                if (num < 0) {
                    printf("\nError: Negative input for real square root.\n");
                } else {
                    Ans = sqrt(num);
                    printf("\nSquare Root = %.4lf\n", Ans);
                }
                break;

            case 7:
                num = get_value("Enter integer for factorial");
                Ans = (double)factorial((int)num);
                printf("\nFactorial Result = %.0lf\n", Ans);
                break;

            case 8:
                num = get_value("Enter value for log10");
                if (num <= 0) {
                    printf("\nLog Error: Value must be positive.\n");
                } else {
                    Ans = log10(num);
                    printf("\nLog10 = %.4lf\n", Ans);
                }
                break;

            case 9:
                num = get_value("Enter value for ln");
                if (num <= 0) {
                    printf("\nLog Error: Value must be positive.\n");
                } else {
                    Ans = log(num);
                    printf("\nNatural Log (ln) = %.4lf\n", Ans);
                }
                break;

            case 10:
                num = get_value("Enter angle in degrees");
                Ans = sin(num * M_PI / 180.0);
                printf("\nSin(%.2lf) = %.4lf\n", num, Ans);
                break;

            case 11:
                num = get_value("Enter angle in degrees");
                Ans = cos(num * M_PI / 180.0);
                printf("\nCos(%.2lf) = %.4lf\n", num, Ans);
                break;

            case 12:
                num = get_value("Enter angle in degrees");
                Ans = tan(num * M_PI / 180.0);
                printf("\nTan(%.2lf) = %.4lf\n", num, Ans);
                break;

            case 13: {
                int n_val = get_int_value("Enter n");
                int r_val = get_int_value("Enter r");
                if (n_val < r_val) {
                    printf("\nError: n cannot be less than r.\n");
                } else {
                    Ans = (double)factorial(n_val) / factorial(n_val - r_val);
                    printf("\nnPr Result = %.0lf\n", Ans);
                }
                break;
            }

            case 14: {
                int n_val = get_int_value("Enter n");
                int r_val = get_int_value("Enter r");
                if (n_val < r_val) {
                    printf("\nError: n cannot be less than r.\n");
                } else {
                    Ans = (double)factorial(n_val) / (factorial(r_val) * factorial(n_val - r_val));
                    printf("\nnCr Result = %.0lf\n", Ans);
                }
                break;
            }

            case 15:
                printf("How many numbers for mean calculation? ");
                scanf("%d", &n);
                result = 0;
                for (int i = 1; i <= n; i++) {
                    sprintf(buffer, "Data point %d", i);
                    result += get_value(buffer);
                }
                Ans = result / n;
                printf("\nCalculated Mean = %.4lf\n", Ans);
                break;

            case 16:
                printf("Comparing how many numbers? ");
                scanf("%d", &n);
                result = get_value("Number 1");
                for (int i = 2; i <= n; i++) {
                    sprintf(buffer, "Number %d", i);
                    num = get_value(buffer);
                    if (num > result) result = num;
                }
                Ans = result;
                printf("\nMaximum Value = %.4lf\n", Ans);
                break;

            case 17:
                printf("Comparing how many numbers? ");
                scanf("%d", &n);
                result = get_value("Number 1");
                for (int i = 2; i <= n; i++) {
                    sprintf(buffer, "Number %d", i);
                    num = get_value(buffer);
                    if (num < result) result = num;
                }
                Ans = result;
                printf("\nMinimum Value = %.4lf\n", Ans);
                break;

            case 18: matrix_add(); break;
            case 19: matrix_multiply(); break;
            case 20: inverse2x2(); break;
            case 21: quadratic(); break;
            case 22: cubic(); break;

            default:
                printf("\nSelection Error: %d is not a valid menu option.\n", choice);
        }
        next_loop:; 
    }
    return 0;
