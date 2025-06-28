# spike-basics
How-To-Use
have a Code or an assembly code ready (.c .S)
for example this matrix mult code :
#include <stdio.h>
#include <stdlib.h>

#define R1 2 // number of rows in Matrix-1
#define C1 2 // number of columns in Matrix-1
#define R2 2 // number of rows in Matrix-2
#define C2 3 // number of columns in Matrix-2

// Multiply a and b without using *
int multiply(int a, int b) {
    int result = 0;
    for (int i = 0; i < b; i++) {
        result += a;
    }
    return result;
}

void multiplyMatrix(int m1[][C1], int m2[][C2])
{
    int result[R1][C2];

    printf("Resultant Matrix is:\n");

    for (int i = 0; i < R1; i++) {
        for (int j = 0; j < C2; j++) {
            result[i][j] = 0;

            for (int k = 0; k < R2; k++) {
                result[i][j] += multiply(m1[i][k], m2[k][j]);
            }

            printf("%d\t", result[i][j]);
        }
        printf("\n");
    }
}

int main()
{
    int m1[R1][C1] = { { 1, 1 }, { 2, 2 } };
    int m2[R2][C2] = { { 1, 1, 1 }, { 2, 2, 2 } };

    if (C1 != R2) {
        printf("The number of columns in Matrix-1 must be "
               "equal to the number of rows in "
               "Matrix-2\n");
        exit(EXIT_FAILURE);
    }

    multiplyMatrix(m1, m2);

    return 0;
}

```bash
# 1. Show source code
cat matrix_mult.c

# 2. Compile 
riscv64-unknown-elf-gcc -g -O0 -o matrix_mult.o matrix_mult.c


you can view it in terminal by doing this :
 riscv64-unknown-elf-objdump -d matrixmult.o | less

or

you can put in a .txt file to open in gvim like this :
 
# 3. Generate and examine object dump
riscv64-unknown-elf-objdump -d matrix_mult.o > obj.txt
gvim obj.txt #(you can use gedit too)
wc -l obj.txt  # Show total lines


4. Extract functions
grep -A 50 "<main>:" obj.txt > main_func.txt
grep -A 30 "<multiply>:" obj.txt > multiply_func.txt  
grep -A 100 "<multiplyMatrix>:" obj.txt > multiplyMatrix_func.txt

# 5. Show function sizes
wc -l *_func.txt

# 6. Generate trace
 spike -l pk matrixmult.o 2>spike.txt

# 7. Show trace size vs object dump size
wc -l trace.log obj.txt
```


