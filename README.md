# Ex-4 Rail-Fence-Program

# IMPLEMENTATION OF RAIL FENCE – ROW & COLUMN TRANSFORMATION TECHNIQUE

# AIM:

# To write a C program to implement the rail fence transposition technique.

# DESCRIPTION:

In the rail fence cipher, the plain text is written downwards and diagonally on successive "rails" of an imaginary fence, then moving up when we reach the bottom rail. When we reach the top rail, the message is written downwards again until the whole plaintext is written out. The message is then read off in rows.

# ALGORITHM:

STEP-1: Read the Plain text.
STEP-2: Arrange the plain text in row columnar matrix format.
STEP-3: Now read the keyword depending on the number of columns of the plain text.
STEP-4: Arrange the characters of the keyword in sorted order and the corresponding columns of the plain text.
STEP-5: Read the characters row wise or column wise in the former order to get the cipher text.

# PROGRAM

```

#include <stdio.h>
#include <string.h>
#include <ctype.h>

int main() {
    char plaintext[500], key[100];
    char matrix[500][100];
    int order[100];
    int len, keyLen, rows;

    printf("Enter Plaintext: ");
    fgets(plaintext, sizeof(plaintext), stdin);
    plaintext[strcspn(plaintext, "\n")] = '\0';

    printf("Enter Keyword: ");
    fgets(key, sizeof(key), stdin);
    key[strcspn(key, "\n")] = '\0';

    len = strlen(plaintext);
    keyLen = strlen(key);

    if (keyLen == 0 || len == 0) {
        printf("Error: Plaintext and key cannot be empty.\n");
        return 1;
    }

    // Convert key to uppercase and validate
    for (int i = 0; i < keyLen; i++) {
        if (!isalpha((unsigned char)key[i])) {
            printf("Error: Key must contain alphabets only.\n");
            return 1;
        }
        key[i] = toupper((unsigned char)key[i]);
        order[i] = i;
    }

    // Remove spaces and convert plaintext to uppercase
    int n = 0;

    for (int i = 0; i < len; i++) {
        if (isalpha((unsigned char)plaintext[i])) {
            plaintext[n++] = toupper((unsigned char)plaintext[i]);
        }
        else if (plaintext[i] != ' ') {
            printf("Error: Plaintext must contain letters and spaces only.\n");
            return 1;
        }
    }

    plaintext[n] = '\0';
    len = n;

    if (len == 0) {
        printf("Error: Plaintext must contain letters.\n");
        return 1;
    }

    // Calculate number of rows
    rows = (len + keyLen - 1) / keyLen;

    // Arrange plaintext row-wise in matrix
    int k = 0;

    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < keyLen; j++) {
            if (k < len) {
                matrix[i][j] = plaintext[k++];
            }
            else {
                matrix[i][j] = 'X';
            }
        }
    }

    // Sort columns according to keyword
    // Stable sorting handles repeated key letters
    for (int i = 0; i < keyLen - 1; i++) {
        for (int j = i + 1; j < keyLen; j++) {
            if (key[order[i]] > key[order[j]]) {
                int temp = order[i];
                order[i] = order[j];
                order[j] = temp;
            }
        }
    }

    // Display matrix
    printf("\nPlaintext Matrix:\n");

    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < keyLen; j++) {
            printf("%c ", matrix[i][j]);
        }
        printf("\n");
    }

    // Display sorted keyword order
    printf("\nSorted Keyword: ");
    for (int i = 0; i < keyLen; i++) {
        printf("%c ", key[order[i]]);
    }

    // Generate ciphertext by reading sorted columns
    printf("\n\nCiphertext: ");

    for (int j = 0; j < keyLen; j++) {
        int col = order[j];

        for (int i = 0; i < rows; i++) {
            printf("%c", matrix[i][col]);
        }
    }

    printf("\n");

    return 0;
}

```

# OUTPUT

<img width="962" height="391" alt="image" src="https://github.com/user-attachments/assets/1f1d17d2-5355-404c-b285-b3adaafe332e" />


# RESULT

Thus the implementation of Railfence Encryption and decryption using C is ran and executed successfully.
