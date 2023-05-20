
/* the game is the hangman game developed in c language using multidimensional arrays and pointers
It is very simple to implement..
developed by Njiu Stephen  # D.C.T */


#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <conio.h>
#include <string.h>
#include <stdlib.h>
#include <time.h>
#include <ctype.h>


#define MAX_WORDS 10
#define MAX_WORDS_LENGTH 80
#define MAX_GUESS 5

int chooseCategory();
char *pickwordfromselectedcategory(char table[MAX_WORDS][MAX_WORDS_LENGTH]);
void hiderandomlyselectedword(char randomlyselectedword[],char hiddenword[]);
int evaluateplayerguess(char guess,char randomlyselectedword[],char hiddenword[]);

void main ()
{
  char kenyantowns [MAX_WORDS][MAX_WORDS_LENGTH]={"KISUMU","MOMBASA","NAIROBI","NYERI","THIKA",
                                                   "NAKURU","MALINDI","KAKAMEGA","MERU","KIAMBU"};

  char asiancountries [MAX_WORDS][MAX_WORDS_LENGTH]={"CHINA","INDIA","KOREA","AFGHANISTAN","JAPAN",
                                                   "BANGLADESH","INDONESIA","PAKISTAN","RUSSIA","NEPAL"};

   char animals [MAX_WORDS][MAX_WORDS_LENGTH]={"LION","ELEPHENT","RHINO","ZEBRA","GIRAFFE","BEER",
                                               "LEOPARD","TIGER","DYNAUSAR","BUFFALO"};


    char selectedCategory[MAX_WORDS][MAX_WORDS_LENGTH]={""};
    char* randomlyselectedword;
    char hiddenword[MAX_WORDS_LENGTH];

    int selectedCategoryindex=-1;
    char guess;
    int wrongguessesbyplayer=0;
    do
    {
      selectedCategoryindex=chooseCategory();
    } while (selectedCategoryindex==-1);

   switch (selectedCategoryindex)
    {
        case 1:
        memcpy(selectedCategory,kenyantowns,sizeof(kenyantowns));
        printf("\n You Have selected Kenyan Towns. Cool, Let's play!.\n");
        break;
        case 2:
                memcpy(selectedCategory,asiancountries,sizeof(asiancountries));

                printf("\n You Have selected Asian Countries. Cool, Let's play!\n");

        break;
        case 3:
        memcpy(selectedCategory,animals,sizeof(animals));
                printf("\n You Have selected Animals. Cool, Let's play!\n");

        break;
        default:
        break;
    }

  //printf("\n");
 // strcpy(randomlyselectedword,selectedCategory[wordstochooseindex]);
 // printf("%s",hiddenword);

  randomlyselectedword=pickwordfromselectedcategory(selectedCategory);

  hiderandomlyselectedword(randomlyselectedword,hiddenword);
  //main GAME LOOP

  while (wrongguessesbyplayer < MAX_GUESS)
  {

    printf("\n\t%s",hiddenword);
    printf("\n\t Enter a character Found in the Hidden word");
    printf("\n\t :");
    scanf("%c",& guess);
    getc(stdin);
    guess=toupper(guess);
    fflush(stdin);
    if(evaluateplayerguess(guess,randomlyselectedword,hiddenword)==0)
    {
      printf("\n Nope! The entered character %c is not a part of the Hidden word!!Please try Again",guess);

      wrongguessesbyplayer++;
    }
    else
    {

        printf("\n BRAVOO! you have entered the correct letter...Great job");
    }
    // the progress of the game
    printf("\n You have %d",MAX_GUESS-wrongguessesbyplayer);
    printf(" Attempts left..\n");
    if(strcmp(randomlyselectedword,hiddenword)==0)
    {
      printf("\n CONGRATULATIONS!! You entered the right word..You have SUCCESSFULLY finished the game..");
      printf("\n The hidden word was: %s",randomlyselectedword);
      printf("\n\n");
      printf(" \t That is it ...Happy Holidays! ");
      break;
    }
  }
     //checking if the player has exceeded the number of guesses allowed
     if(wrongguessesbyplayer==MAX_GUESS)
     {
      printf("\n OOOOH TOO BAD. You have been HANGED \n");
      printf("\n The hidden word was %s! ",randomlyselectedword);
      printf("\n Play again\n\n");
     }


 getch();


}
int chooseCategory()
{   
  printf("\n\n\t # Welcome to Hangman game..\n");
  printf("\n\t #How to play?");
  printf("\n\t #Select one of the category and based on your knowledge predict the hidden word ");
  printf("\n\t # Hint: The No. of stars are equal to the No. of Letters");
  printf("\n\t # Are you ready?? Let's Go\n\n");
  printf("\t-----------------------------------------------------\n");

  printf("\t Select a category of words to play with... \n");
    printf(" \t Press 1 to choose 'Kenyan Towns\n'");
        printf(" \t Press 2 to choose Asian Countries \n");
    printf(" \t Press 3 to choose Animals \n\t :");
    int userinput;
    scanf("%d",&userinput);
    getc(stdin);
    fflush(stdin);
    if(userinput>0 && userinput<4)
    {
        return userinput;
    }
    else
    {
        printf("\n Invalid Choice. Please try again...\n\n");
             return -1;

    }

}
char* pickwordfromselectedcategory( char table[MAX_WORDS][MAX_WORDS_LENGTH])
{
srand(time(NULL));
    int wordstochooseindex=rand() % MAX_WORDS;
    // you can hack the program at this point to get the answer quickly....
    return (char*)table[wordstochooseindex];
}

void hiderandomlyselectedword(char randomlyselectedword[],char hiddenword[])
{
    int index;
    int wordlength;
    wordlength=strlen(randomlyselectedword);
    if(wordlength > 3)
    for(index=0;index < wordlength;index++)
    {
    hiddenword[index]='*';
    }
  hiddenword[index]='\0';
}

int evaluateplayerguess(char guess,char randomlyselectedword[],char hiddenword[])
{
int index;
int outcome=0;
//check if the word has already been guessed and hence complete the game;
for(index=0;randomlyselectedword[index] !='\0';index++)
{
    if(guess==hiddenword[index])
    {
       return 0;
    }
  //check if the player has guessed the right word
    if(guess==randomlyselectedword[index])
      {
        hiddenword[index]=guess;
          outcome++;
      }

}
      return outcome;

}

