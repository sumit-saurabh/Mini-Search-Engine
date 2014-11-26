/*                               MINI SEARCH ENGINE
---------------------------------------------------------------------------------------------
Created on: 09/01/2013
-------------------------------------------------------------------------------------------
*/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#define size_of_index 50000
#define num_of_char 1000000
#define final_result_num 100
#define display_line 3
#define till_this_space 5
#define ON '1'
#define OFF '2'
#define CLICK_SOUND if(sound_set==ON){printf("\a");}
#define SETTINGS if(ch==19){settings();}  // press crtl+s for settings

typedef struct buffer_store  // making prog more flexible by removing no. of inputs restriction of array
{
    int page_num;
    char page_content[num_of_char];
    struct buffer_store *link_next_page;
}buffer;

typedef struct storage    // creating dynamic chunks of space for storing indices of related words from buffer
{
    int storage_num;
    char *index[size_of_index];
    char *initial_index[size_of_index];
    struct storage *link_next_storage;
}index_storage;

char *final_result[final_result_num];  // its storing starting address of lines containing particular result
char *patern_in_line[final_result_num];

static int counter = 0;
static int count =0;
static int storage_no = 1;
static int page_no = 1;
static int result_no = 0;
static int occur = 0;
char sound_set = OFF; // defaault click sound
char search_opt = OFF;

void load_on_buffer(buffer*);
void creat_page(buffer**);
void creat_index_storage(index_storage**);
void search(buffer**, char*, index_storage**);
void user_input(char* ,buffer**, index_storage**);
void display_result(index_storage**);
void show_brief(index_storage*,int);
void top_results(index_storage**);
void show_detail(int);
void settings();
void choose_index(index_storage**);

void main()
{
    int i;
    char pat[50],ch;   // pat = pattern;

    // Creating and intializing base page and current page
    buffer *base_page =NULL, *current_page;
    creat_page(&base_page);
    current_page = base_page;

    // Creating and initializeing index storage
    index_storage *base_storage = NULL,*current_storage;
    creat_index_storage(&base_storage);
    current_storage = base_storage;

    // Opening file and storing its contents in buffer (RAM) for faster work
    load_on_buffer(current_page);
    // loadind of file content on the buffer (RAM) is over

    // testing of loading by printing
  current_page=base_page;int choice;
    while(current_page!=NULL)
    {
        printf("\npage no. %d\n",current_page->page_num);
        for(choice=0; choice < num_of_char; choice++)
        {
            if(current_page->page_content[choice]=='#')
                break;
            printf("%c",current_page->page_content[choice]);
        }
        current_page=current_page->link_next_page;
    }

    // user part starts from here
    user_input(pat,&base_page,&base_storage);
    // user entry is over

    // top results is collected from storage
    top_results(&base_storage);

    // user entry for choosing result's index
    choose_index(&base_storage);
    printf("\n\n\t\t\tTHANK YOU FOR USING THIS SOFTWARE %c\n",1);
    printf("\nPress any key for exit");
    ch = getche();
}

void settings()
{
    system("cls");
    char ch='y';
    while(ch=='y')
    {
        printf("\nSettings:\n");
        printf("\n1.Click Sound Setting:\t\t Status: ");
        (sound_set==ON)? printf("ON\n"):printf("OFF\n");
        printf("\n2.Result display Setting:\t Status: ");
        (search_opt==ON)? printf("All Search\n"):printf("Comprehensive search\n");
        ch = getche();
        CLICK_SOUND
        system("cls");
        switch(ch)
        {
            case '1':
            printf("\n1.Click Sound Setting:\t1.ON\t2.OFF\n");
            sound_set = getche();
            CLICK_SOUND
            system("cls");
            printf("\nClick Sound Status: ");
            (sound_set==ON)? printf("ON\n"):printf("OFF\n");
            break;

            case '2':
            printf("\nRESULT DISPLAY SETTING:\t   1.All search\t   2.Comprehensive search\n");
            search_opt = getche();
            CLICK_SOUND
            system("cls");
            printf("\nResult Display Status: ");
            (search_opt==ON)? printf(" All search\n"):printf(" Comprehensive search\n");
            break;
        }
        printf("\nWant to perform more settings...y/n\n");
        ch = getche();
        CLICK_SOUND
        system("cls");
    }
}

void user_input(char *pat, buffer **base_page, index_storage **base_storage)
{
    char ch;
    int i=-1;
    printf("\nSEARCH FOR:\n");
    while(1)
    {
        count=0;
        ch = getche();
        CLICK_SOUND // for sound effect during input click

        if(ch==19)     // for clear
        system("cls");

        SETTINGS
        if(ch==13)     // 13 is ASCII value for ENTER and 10 is for \n
         {
            system("cls");
            printf("\nSEARCH FOR:\n");
            puts(pat);
            if(i>=0)
            search(base_page, pat, base_storage);
            break;
         }
        system("cls");
        if(ch=='\b')
        {
            pat[i]='\0';
            i--;
        }
        else if(ch!=19)
        {
            i++;
            pat[i]=ch;
            pat[i+1]='\0';
        }
        printf("\nSEARCH FOR:\n");
        puts(pat);
        if(i>=0)
            search(base_page, pat, base_storage);
       if(i<0)
            i=-1;
    }
}

void load_on_buffer(buffer* current_page)
{
    char ch;
    FILE *fp;
    fp = fopen("DATA_BASE.txt","r");
    if(fp==NULL)
    {
        puts("\n\n\n\n\t\t\t     Oops! DATABASE NOT FOUND\n\t\t\t\tLink failure occur.\n\t\tPlease keep Database and .exe file in the same package.\n");
        ch = getche();
        exit(1);
    }
    while(1)
    {
        ch = getc(fp);
        if(ch==EOF)
            break;
        if(counter==num_of_char)
            creat_page(&current_page);  // new page is created
        current_page->page_content[counter] = ch;
        counter++;
    }
    current_page->page_content[counter] = '#';   // '#' => end of contents
}

void creat_page(buffer **old_page)
{
    buffer *new_page = (buffer*)malloc(sizeof(buffer));
    new_page->link_next_page = NULL;
    new_page->page_num = page_no;
    page_no++;
    if(*old_page != NULL)
        (*old_page)->link_next_page = new_page;
    *old_page=new_page;
    counter=0;
}

void creat_index_storage(index_storage **old_index_storage)
{
    index_storage *new_index_storage;
    new_index_storage = (index_storage*)malloc(sizeof(index_storage));
    new_index_storage->link_next_storage = NULL;
    new_index_storage->storage_num = storage_no;
    storage_no++;
    if(*old_index_storage!=NULL)
        (*old_index_storage)->link_next_storage = new_index_storage;
    *old_index_storage = new_index_storage;
    counter=0;
}

void search(buffer **base_page,char *pat, index_storage **base_storage)
{
    int len_pat = strlen(pat), i=0, j, k, found=1;
    occur=0;
    buffer *page = *base_page;
    index_storage *current_storage = *base_storage;
    counter=0;
    current_storage->initial_index[counter]=page->page_content; // initially storing address of first line of buffer page
    while(page->page_content[i]!='#')
    {
        if(counter==size_of_index)
                creat_index_storage(&current_storage);                    //new storage created

        if((page->page_content[i]=='.')||(page->page_content[i]=='?'))    // storing starting address of line containing pattern
            current_storage->initial_index[counter]= page->page_content+i+1;

        k = 0;                      // k counter is used for fragmented word between two page
        for(j=0;j<len_pat;j++)      // scanning texts from page
        {
            if((i+j)>=num_of_char)  // this code manages right search for fragmented words
            {
                if(page->link_next_page->page_content[k] != (toupper(pat[j])||tolower(pat[j])))
                {
                    found = 0;
                    break;
                }
                k++;
            }
            else if(page->page_content[i+j]!=toupper(pat[j]) && page->page_content[i+j]!=tolower(pat[j]))  // during scanning page, if text being scanned doesnt match to pattern, then come out of loop
            {
                found=0;
                break;
            }
        }
        if(found==1)  // here OFF => comprehensive search
        {
            if((search_opt==OFF)&&(page->page_content[i-1]==' '||page->page_content[i-1]=='-')||i==0)
            {
                current_storage->index[counter]= page->page_content+i;
                counter++;
                count++;
                occur=1;
            }
            else if(search_opt==ON)   // here ON => all search
            {
                current_storage->index[counter]= page->page_content+i;
                //  printf("\nPattern found in page no. %d and index no %d\n",page->page_num,i);
                counter++;
                count++;
                occur=1;
            }
        }
        found=1;
        i++;
        if(num_of_char==i)
        {
            page=page->link_next_page; // page change
            i=0;  // resetting the counter i, when page is changed
        }
    }
    printf("\nshowing %d results\n",count);
    // displaying the result
    display_result(base_storage);
    // end of display
    return;
}
/*
    space for correction algo

*/

void display_result(index_storage **base_storage)
{
    int choice;
    index_storage *current_storage=*base_storage;
    result_no=1;  // index showing in result is resetting
    printf("\nShowing result for:\n");
    while(current_storage!=NULL)
    {
        for(choice=0; choice < size_of_index; choice++)
        {
            if((current_storage->link_next_storage==NULL)&&(choice==counter))
                break;
            if(choice>=count)
                break;
            show_brief(current_storage,choice);
        }
        current_storage=current_storage->link_next_storage;
    }
    //
    if(occur==0)
        printf("\nSorry! Result NOT found\n");
}

void show_brief(index_storage *current_storage,int choice)
{
    int i=0,j=0;
    index_storage *next;
    printf("%d. ",result_no++);
    while(*((current_storage->index[choice])+i)!='.')
    {
        if(*((current_storage->index[choice])+i)==' ')
            j++;
        if(*((current_storage->index[choice])+i)==','||(j==till_this_space))
            break;
        if(*((current_storage->index[choice])+i)!='\n')
        printf("%c",*((current_storage->index[choice])+i));
        i++;
    }
    printf("\n");
}

void top_results(index_storage **base_storage)
{
    index_storage *current_storage = *base_storage;
    for(counter=0; counter<final_result_num; counter++)
    {
        if(counter>count)
            break;
        final_result[counter]=current_storage->initial_index[counter];
        patern_in_line[counter]=current_storage->index[counter];
    }
}

void show_detail(int result_no)
{
    int i=0,j=0,flag=0;
    while(1)
    {
        if((*(final_result[result_no-1]+i-1)=='.')&&((*(final_result[result_no-1]+i)=='\n')||(*(final_result[result_no-1]+i)=='#')))
            break;
        if((final_result[result_no-1]+i) == patern_in_line[result_no-1])
            flag=till_this_space;
        if(flag>0)    // In this case, key word will be printed in upper case
        {
            if((*(final_result[result_no-1]+i)==',')||(*(final_result[result_no-1]+i)=='.'))
                flag=0;
            else if(*(final_result[result_no-1]+i)==' ')
                flag--;
            printf("%c",toupper(*((final_result[result_no-1])+i)));
        }
        else
            printf("%c",*((final_result[result_no-1])+i));
        i++;
    }
}

void choose_index(index_storage **base_storage)
{
    printf("\nCHOOSE:\n");
    char ch = getche();
    CLICK_SOUND
    while(1)
    {
        SETTINGS
        else if( ch-48 >=1 && ch-48 <=count)
        {
            printf("\n");
            show_detail(ch-48);
            break;
        }
        else
        {
            printf("\nOops! Input didnot matched with any Index\nTry Again\n");
        }
        if(ch==19)
        {
            display_result(base_storage);
            printf("\nChoose:\n");
        }
        else
        printf("\nCHOOSE CORRECT RESULTS'S INDEX:\n\n");
            ch = getche();
            CLICK_SOUND;
    }
    printf("\n\n");
}
