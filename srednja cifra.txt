#include<iostream>
using namespace std;
void unos(int[], int);
int brojCifara(int);
float srednjacifra(int n);
void incijalizacija(int [],float [],int);
int main()
{
	int niz[20] = {};
	float nizrez[20] = {};
	int n;

	do
	{
		cout << "Unesite vrijednost n: ";
		cin >> n;

	} while (n < 1 || n>20);
	unos(niz, n);
	incijalizacija(niz, nizrez, n);
	system("pause>0");
	return 0;
}
void unos(int niz[], int n)
{
	for (int i = 0;i < n;i++)
	{
		do
		{
			cout << "Unesite " << i + 1 << " element niza: ";
			cin >> niz[i];
		} while (niz[i] < 100 || niz[i] % 2 == 0);

	}
}
int brojCifara(int br)
{
	int brojac = 0;
	while (br > 0)
	{
		br /= 10;
		brojac++;
	}
	return brojac;
}
float srednjacifra(int n)
{
	float rezultat;
	int temp = 0;
	int brcfr = brojCifara(n);
	if (brcfr % 2 != 0)
	{
		brcfr /= 2;
		rezultat = n / pow(10, brcfr);
		return int( rezultat) % 10;
	}
	else
	{
		brcfr /= 2;
		rezultat =(int( n / pow(10, brcfr)))%10;
	
		temp = (int(n / pow(10, brcfr-1)))%10;
		rezultat = (rezultat + temp) / 2;
        return rezultat;
	}
	
}

void incijalizacija(int niz[], float nizrez[], int n)
{
	for (int i = 0;i < n;i++)
	{
		nizrez[i] = srednjacifra(niz[i]);
		cout << nizrez[i] << endl;
	}
}
