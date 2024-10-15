#define _SECURE_NO_WARNINGS
#include<stdio.h>
#include<stdlib.h>   
#include<string.h>
#define ERROR_OPENING_FILE -1
#define MAX_BR_BODOVA 100
#define BUFFER_SIZE 1024
typedef struct {
	char ime[30];
	char prezime[30];
	float bodovi;
}student;

int broji_redove(char* ime_datoteke);

void upis_iz_datoteke(char* ime_datoteke, student* s, int broj_studenata);

void ispis_strukture(student* s, int broj_studenata);

int main()
{
	student* s = NULL;
	const char* ime_datoteke = "studenti.txt";
	
	int broj_studenata = broji_redove(ime_datoteke);
	
	//Alokacija memorije za strukturu
	s = (student*)malloc(broj_studenata * sizeof(student));
	if (s == NULL)
	{
		printf("Pogreska prilikom alokacije memorije!\n");
		return ERROR_OPENING_FILE;
	}
	
	upis_iz_datoteke(ime_datoteke, s,  broj_studenata);
	
	printf("Ispis podataka studenata:\n");
	ispis_strukture( s, broj_studenata);
	
	free(s);
	
	return 0;
}
int broji_redove(char* ime_datoteke)
{
	char buffer[BUFFER_SIZE];
	int brojac = 0;

	FILE *p=NULL;
	fopen(ime_datoteke, "r");
	if (p == NULL)
	{
		printf("Pogreska prilikom otvaranja datoteke!\n");
		return ERROR_OPENING_FILE;
	}
	
	while (!feof(p))
	{
		fgets(buffer, BUFFER_SIZE, p);
		brojac++;
	}
	fclose(p);
	return brojac;
}
void upis_iz_datoteke(char* ime_datoteke, student* s, int broj_studenata)
{
	FILE* p=NULL;
	fopen(ime_datoteke, "r");
	if (p == NULL)
	{
		printf("Pogreska prilikom otvaranja datoteke!\n");
		return ERROR_OPENING_FILE;
	}
	
	for (int i = 0; i < broj_studenata; i++)
	{
		fscanf(p, "%s %s %f", s[i].ime, s[i].prezime, &s[i].bodovi);
	}
	fclose(p);
}
void ispis_strukture(student* s, int broj_studenata)
{
	for (int i = 0; i < broj_studenata; i++)
	{
		printf("Podatci %d studenta:\n",i+1);
		printf("Ime i prezime: %s %s\n", s[i].ime, s[i].prezime);
		printf("Apsolutni broj bodova: %.3f\n", s[i].bodovi);
		//racunanje relativnog broja bodova svakog pojedinog studenta
		float relativan_broj_bodova = (float)s[i].bodovi / MAX_BR_BODOVA * 100;
		printf("Relativan broj bodova: %.3f\n", relativan_broj_bodova);
	}
}

