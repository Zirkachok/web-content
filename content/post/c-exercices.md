+++
menu        = ""
date        = "2017-03-24T15:44:17+01:00"
title       = "Quelques exercices en C"
description = ""
categories  = []
tags        = []
images      = []
banner      = ""
draft       = true
+++

<!-- 

//#include  <iostream.h>
//#include  <conio.h>
#include  <stdio.h>


 char  *c[] = {
    "SCHILLER",
    "JULIEN"  ,
    "TODOR",
    "FRANCE"
    } ;

/// SCHILLER
/// JULIEN
/// TODOR
/// FRANCE

char  **cp[] =  { c+3,c+2,c+1,c };



char ***cpp = cp;

void main(void)
{
  //clrscr();

  printf("%s",  **++ cpp ); //TODOR
  printf("%s ", *--*(++ cpp) ); // SCHILLER
  printf("%s",  *cpp[-2] ); // FRANCE

  //char S = getch();

}

 -->