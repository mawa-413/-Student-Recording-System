#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_STUDENTS 10

typedef struct {
    char name[50];
    char id[20];
    int age;
    float gpa;
} Student;

Student students[MAX_STUDENTS];
int studentCount = 0;

void addOrUpdateStudent() {
    char id[20];
    printf("Enter Student ID: ");
    scanf("%s", id);

    int found = -1;
    for (int i = 0; i < studentCount; i++) {
        if (strcmp(students[i].id, id) == 0) {
            found = i;
            break;
        }
    }

    if (found == -1) {
        if (studentCount < MAX_STUDENTS) {
            strcpy(students[studentCount].id, id);
            printf("Enter Name: ");
            scanf("%s", students[studentCount].name);
            printf("Enter Age: ");
            scanf("%d", &students[studentCount].age);
            printf("Enter GPA: ");
            scanf("%f", &students[studentCount].gpa);
            studentCount++;
        } else {
            printf("Student records are full.\n");
        }
    } else {
        printf("Enter New Name: ");
        scanf("%s", students[found].name);
        printf("Enter New Age: ");
        scanf("%d", &students[found].age);
        printf("Enter New GPA: ");
        scanf("%f", &students[found].gpa);
    }
}

void displayStudents() {
    printf("ID\t\tName\t\tAge\tGPA\n");
    for (int i = 0; i < studentCount; i++) {
        printf("%s\t%s\t%d\t%.2f\n", students[i].id, students[i].name, students[i].age, students[i].gpa);
    }
}

void searchStudent() {
    char id[20];
    printf("Enter Student ID to search: ");
    scanf("%s", id);

    int found = -1;
    for (int i = 0; i < studentCount; i++) {
        if (strcmp(students[i].id, id) == 0) {
            found = i;
            break;
        }
    }

    if (found != -1) {
        printf("Student Found: %s, Age: %d, GPA: %.2f\n", students[found].name, students[found].age, students[found].gpa);
    } else {
        printf("Student not found.\n");
    }
}

void saveRecords() {
    FILE *file = fopen("students.dat", "wb");
    if (file != NULL) {
        fwrite(&studentCount, sizeof(int), 1, file);
        fwrite(students, sizeof(Student), studentCount, file);
        fclose(file);
        printf("Records saved successfully.\n");
    } else {
        printf("Error saving records.\n");
    }
}

void loadRecords() {
    FILE *file = fopen("students.dat", "rb");
    if (file != NULL) {
        fread(&studentCount, sizeof(int), 1, file);
        fread(students, sizeof(Student), studentCount, file);
        fclose(file);
    } else {
        printf("No saved records found.\n");
    }
}

int main() {
    int choice;
    loadRecords();

    while (1) {
        printf("\n1. Add/Update Student\n");
        printf("2. Display Students\n");
        printf("3. Search Student\n");
        printf("4. Save Records\n");
        printf("5. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1: addOrUpdateStudent(); break;
            case 2: displayStudents(); break;
            case 3: searchStudent(); break;
            case 4: saveRecords(); break;
            case 5: exit(0); break;
            default: printf("Invalid choice, try again.\n"); break;
        }
    }

    return 0;
}
