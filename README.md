# HILL CIPHER
HILL CIPHER
EX. NO: 3 AIM:
 

IMPLEMENTATION OF HILL CIPHER
 
## To write a C program to implement the hill cipher substitution techniques.

## DESCRIPTION:

Each letter is represented by a number modulo 26. Often the simple scheme A = 0, B
= 1... Z = 25, is used, but this is not an essential feature of the cipher. To encrypt a message, each block of n letters is  multiplied by an invertible n × n matrix, against modulus 26. To
decrypt the message, each block is multiplied by the inverse of the m trix used for
 
encryption. The matrix used
 
for encryption is the cipher key, and it sho
 
ld be chosen
 
randomly from the set of invertible n × n matrices (modulo 26).


## ALGORITHM:
STEP-1: Read the plain text and key from the user. 

STEP-2: Split the plain text into groups of length three.

STEP-3: Arrange the keyword in a 3*3 matrix.

STEP-4: Multiply the two matrices to obtain the cipher text of length three.

STEP-5: Combine all these groups to get the complete cipher text.

## PROGRAM
```
#include <ctype.h>
#include <stdio.h>

#define MAX_TEXT 4096

static int gcd(int a, int b)
{
	while (b != 0) {
		int remainder = a % b;
		a = b;
		b = remainder;
	}
	return a;
}

int main(void)
{
	char plaintext[MAX_TEXT];
	char key_text[128];
	char letters[MAX_TEXT];
	int key[3][3];
	int key_length = 0;
	int letter_count = 0;
	int determinant;

	printf("Enter plaintext: ");
	if (fgets(plaintext, sizeof plaintext, stdin) == NULL) {
		fprintf(stderr, "Could not read plaintext.\n");
		return 1;
	}

	printf("Enter the 9-letter key: ");
	if (fgets(key_text, sizeof key_text, stdin) == NULL) {
		fprintf(stderr, "Could not read key.\n");
		return 1;
	}

	for (int i = 0; key_text[i] != '\0'; ++i) {
		if (isalpha((unsigned char)key_text[i])) {
			if (key_length == 9) {
				fprintf(stderr, "The key must contain exactly 9 letters.\n");
				return 1;
			}
			key[key_length / 3][key_length % 3] =
				toupper((unsigned char)key_text[i]) - 'A';
			++key_length;
		}
	}
	if (key_length != 9) {
		fprintf(stderr, "The key must contain exactly 9 letters.\n");
		return 1;
	}

	determinant = key[0][0] * (key[1][1] * key[2][2] - key[1][2] * key[2][1])
				- key[0][1] * (key[1][0] * key[2][2] - key[1][2] * key[2][0])
				+ key[0][2] * (key[1][0] * key[2][1] - key[1][1] * key[2][0]);
	determinant %= 26;
	if (determinant < 0)
		determinant += 26;
	if (gcd(determinant, 26) != 1) {
		fprintf(stderr, "The key matrix is not invertible modulo 26.\n");
		return 1;
	}

	for (int i = 0; plaintext[i] != '\0'; ++i) {
		if (isalpha((unsigned char)plaintext[i]))
			letters[letter_count++] = (char)toupper((unsigned char)plaintext[i]);
	}
	if (letter_count == 0) {
		fprintf(stderr, "Plaintext must contain at least one letter.\n");
		return 1;
	}
	while (letter_count % 3 != 0)
		letters[letter_count++] = 'X';

	printf("Ciphertext: ");
	for (int i = 0; i < letter_count; i += 3) {
		int block[3];
		for (int row = 0; row < 3; ++row) {
			int value = 0;
			for (int col = 0; col < 3; ++col)
				value += key[row][col] * (letters[i + col] - 'A');
			block[row] = value % 26;
		}
		for (int row = 0; row < 3; ++row)
			putchar('A' + block[row]);
	}
	putchar('\n');
	return 0;
}
```

## OUTPUT
<img width="274" height="107" alt="image" src="https://github.com/user-attachments/assets/f30da8c1-486a-4034-985a-c8845130deb4" />
## RESULT
In conclusion, the Hill cipher implementation was executed successfully.
