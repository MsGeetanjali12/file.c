#include <stdio.h>
#include <stdlib.h>
#include <string.h>
struct employee
{
    char name[50];
    float salary;
    int age;
    int id;
};
struct employee e;
long int size = sizeof(e);
FILE *fp, *ft;
void addRecord()
{
    fseek(fp, 0, SEEK_END);
    printf("\nEnter Employee name : ");
    scanf("%s", e.name);
    printf("\nEnter Employee's age : ");
    scanf("%d", &e.age);
    printf("\nEnter monthly salary : ");
    scanf("%f", &e.salary);
    printf("\nEnter Employee id: ");
    scanf("%d", &e.id);
    fwrite(&e, size, 1, fp);
}

void displayRecord()
{
    rewind(fp);
    while (fread(&e, size, 1, fp) == 1)
    {
        printf("\n%s\t%d\t%.2f\t%10d",e.name, e.age, e.salary, e.id);
        printf("\n");
    }
}

void modifyRecord()
{
    char empname[50];
    printf("\nEnter employee name to modify : ");
    scanf("%s", empname);
    rewind(fp);
    while (fread(&e, size, 1, fp) == 1)
    {
        if (strcmp(e.name, empname) == 0)
        {
            printf("\nEnter new name:");
            scanf("%s", e.name);
            printf("\nEnter new age :");
            scanf("%d", &e.age);
            printf("\nEnter new monthly salary :");
            scanf("%f", &e.salary);
            printf("\nEnter new Employee id :");
            scanf("%d", &e.id);
            fseek(fp, -size, SEEK_CUR);
            fwrite(&e, size, 1, fp);
            break;
        }
    }
}

void deleteRecord()
{
    char empname[50];
    printf("\nEnter Employee name to delete : ");
    scanf("%s", empname);
    ft = fopen("temp.txt", "wb");
    rewind(fp);
    while (fread(&e, size,1, fp) == 1)
    {
        if (strcmp(e.name,empname) != 0)
            fwrite(&e, size, 1, ft);
    }
    fclose(fp);
    fclose(ft);
    remove("data.txt");
    rename("temp.txt", "data.txt");
    fp = fopen("data.txt", "rb+");
}
void searchByName()
{
    int option;
    int on = 0;
    printf("1.search by name\n2.search by salary\n3.search by age\n4.search by id\nenter your option\n");
    scanf("%d", &option);
    switch (option)
    {
    case 1:
    {
        char empname2[50];
        printf("\nEnter employee name to search : ");
        scanf("%s", empname2);
        rewind(fp);

        while (fread(&e, size, 1, fp) == 1)
        {
            if (strcmp(e.name, empname2) == 0)
                printf("\n%s\t\t%d\t\t%.2f\t%10d", e.name, e.age, e.salary, e.id);
            on = on + 1;
        }
    }
    break;

    case 2:
    {
        int emps;
        int k;
        int i;
        printf("\nEnter employee salary to search : ");
        scanf("%d", &emps);
        rewind(fp);

        while (fread(&e, size, 1, fp) == 1)
        {
            k = e.salary;

            if (k == emps)
            {
                on = on + 1;
                printf("\n%s\t\t%d\t\t%.2f\t%10d", e.name, e.age, e.salary, e.id);
            }
        }
    }
    break;
    case 3:
    {
        int emps;
        int k;
        int i;
        printf("\nEnter employee age to search : ");
        scanf("%d", &emps);
        rewind(fp);

        while (fread(&e, size, 1, fp) == 1)
        {
            k = e.age;

            if (k == emps)
            {
                on = on + 1;
                printf("\n%s\t\t%d\t\t%.2f\t%10d", e.name, e.age, e.salary, e.id);
            }
        }
    }
    break;
    case 4:
    {

        int emps;
        int k;
        int i;
        printf("\nEnter employee id to search : ");
        scanf("%d", &emps);
        rewind(fp);

        while (fread(&e, size, 1, fp) == 1)
        {
            k = e.id;

            if (k == emps)
            {
                on = on + 1;
                printf("\n%s\t\t%d\t\t%.2f\t%10d", e.name, e.age, e.salary, e.id);
            }
        }
    }
    break;
    default:
        printf("invalid option....\n");
        on=on+1;
        break;
    }
    if (on == 0)
    {
        printf("The given search item cannot be matched with any record");
    }
}

int main()
{
    int choice;
    fp = fopen("data.txt", "rb+");
    if (fp == NULL)
    {
        fp = fopen("data.txt", "wb+");
    }
    if (fp == NULL)
    {
        printf("\nfile cannot be opened...");
        exit(1);
    }

    while (1)
    {
        printf("\n1.___ADD RECORD___\n");
        printf("\n2.___DISPLAY RECORD___\n");
        printf("\n3.___MODIFY RECORDS___\n");
        printf("\n4.___DELETE RECORD___\n");
        printf("\n5.___SEARCH___\n");
        printf("\n6.__EXIT___\n");
        printf("\nENTER YOUR CHOICE:\n");
        fflush(stdin);
        scanf("%d", &choice);
        switch (choice)
        {
        case 1:

            addRecord();
            break;

        case 2:
            displayRecord();
            break;

        case 3:
            modifyRecord();
            break;

        case 4:
            deleteRecord();
            break;

        case 5:
            searchByName();
            break;

        case 6:
            fclose(fp);
            exit(0);
            break;

        default:
            printf("\n____INVALID CHOICE____\n");
        }
    }

    return 0;
}
