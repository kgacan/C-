#include<iostream>
using namespace std;

/****************************************************************************
1. SVE KLASE TREBAJU POSJEDOVATI ADEKVATAN DESTRUKTOR
2. NAMJERNO IZOSTAVLJANJE KOMPLETNIH I/ILI POJEDINIH DIJELOVA DESTRUKTORA KOJI UZROKUJU RUNTIME ERROR CE BITI OZNACENO KAO "RE"
3. SPASAVAJTE PROJEKAT KAKO BI SE SPRIJECILO GUBLJENJE URADJENOG ZADATKA
4. PROGRAMSKI CODE SE TAKODJER NALAZI U FAJLU CODE_.TXT
****************************************************************************/
template<class T1, class T2>
class Kolekcija {
	T1 * _elementi1;
	T2 * _elementi2;
	int _trenutnoElemenata;
public:
	Kolekcija()
	{
		_elementi1 = nullptr;
		_elementi2 = nullptr;
		_trenutnoElemenata = 0;
	}

	Kolekcija(const Kolekcija& k)
	{
		_elementi1 = new T1[k._trenutnoElemenata];
		_elementi2 = new T2[k._trenutnoElemenata];

		for (int i = 0; i < k._trenutnoElemenata; i++)
		{
			_elementi1[i] = k._elementi1[i];
			_elementi2[i] = k._elementi2[i];

		}

		_trenutnoElemenata = k._trenutnoElemenata;

	}

	Kolekcija& operator=(const Kolekcija& k)
	{
		if (this != &k)
		{
			delete[] _elementi1;
			delete[] _elementi2;

			_elementi1 = new T1[k._trenutnoElemenata];
			_elementi2 = new T2[k._trenutnoElemenata];

			for (int i = 0; i < k._trenutnoElemenata; i++)
			{
				_elementi1[i] = k._elementi1[i];
				_elementi2[i] = k._elementi2[i];

			}

			_trenutnoElemenata = k._trenutnoElemenata;
		}

		return *this;
	}

	~Kolekcija()
	{
		delete[] _elementi1;
		_elementi1 = nullptr;
		delete[] _elementi2;
		_elementi2 = nullptr;
	}

	bool AddElement(T1 el1, T2 el2)
	{
		for (int i = 0; i < _trenutnoElemenata; i++)
		{
			if (_elementi1[i] == el1 && _elementi2[i] == el2)//operator== za T1 i T2
				return false;
		}

		T1* temp1 = new T1[_trenutnoElemenata + 1];//defaultn-i kons.
		T2* temp2 = new T2[_trenutnoElemenata + 1];

		for (int i = 0; i < _trenutnoElemenata; i++)
		{
			temp1[i] = _elementi1[i];//operator=
			temp2[i] = _elementi2[i];

		}

		delete[] _elementi1;
		delete[] _elementi2;

		_elementi1 = temp1;
		_elementi2 = temp2;

		_elementi1[_trenutnoElemenata] = el1;
		_elementi2[_trenutnoElemenata++] = el2;

		return true;

	}

	void RemoveElement(int index)
	{
		if (index >= 0 && index < _trenutnoElemenata)
		{
			T1* temp1 = new T1[_trenutnoElemenata - 1];//defaultn-i kons.
			T2* temp2 = new T2[_trenutnoElemenata - 1];

			for (int i = 0, j = 0;  i < _trenutnoElemenata; i++)
			{
				if (i != index)
				{
					temp1[j] = _elementi1[i];//operator=
					temp2[j++] = _elementi2[i];
				}
			}

			delete[] _elementi1;
			delete[] _elementi2;

			_elementi1 = temp1;
			_elementi2 = temp2;
			_trenutnoElemenata--;
		}
	}

	T1& operator[](int index)
	{
		return _elementi1[index];
	}

	T2& GetElement2(int index)
	{
		return _elementi2[index];
	}

	int getTrenutno()const { return _trenutnoElemenata; }

	friend ostream& operator<<<>(ostream&, const Kolekcija&);
};

template<class T1, class T2>
ostream& operator<<(ostream& cout, const Kolekcija<T1,T2>& k)
{
	for (int i = 0; i < k._trenutnoElemenata; i++)
	{
		cout << k._elementi1[i] << endl;//T1 treba imati preklopljen operator<<
		cout << k._elementi2[i] << endl;//T2 treba imati preklopljen operator<<
	}

	return cout;
}

class Datum {
	int *_dan, *_mjesec, *_godina;
public:
	Datum(int dan = 1, int mjesec = 1, int godina = 2000) {
		_dan = new int(dan);
		_mjesec = new int(mjesec);
		_godina = new int(godina);
	}

	Datum(const Datum& d)
	{
		_dan = new int(*d._dan);
		_mjesec = new int(*d._mjesec);
		_godina = new int(*d._godina);
	}

	~Datum() {
		delete _dan; _dan = nullptr;
		delete _mjesec; _mjesec = nullptr;
		delete _godina; _godina = nullptr;
	}

	Datum& operator=(const Datum& d)
	{
		if (this != &d)
		{
			*_dan = *d._dan;
			*_mjesec = *d._mjesec;
			*_godina = *d._godina;
		}

		return *this;
	}

	friend bool operator==(const Datum&, const Datum&);

	friend ostream& operator<< (ostream &COUT, const Datum& obj) {
		COUT << *obj._dan << " " << *obj._mjesec << " " << *obj._godina;
		return COUT;
	}
};

bool operator==(const Datum& d1, const Datum& d2)
{
	return *d1._dan == *d2._dan &&  *d1._mjesec == *d2._mjesec && *d1._godina == *d2._godina;
}
class Student {
	char * _imePrezime;
	Datum * _datumRodjenja;
public:
	Student(char * imePrezime = "---", Datum d = Datum()) {
		int size = strlen(imePrezime) + 1;
		_imePrezime = new char[size];
		strcpy_s(_imePrezime, size, imePrezime);
		_datumRodjenja = new Datum(d);
	}

	Student(const Student& s)
	{
		int size = strlen(s._imePrezime) + 1;
		_imePrezime = new char[size];
		strcpy_s(_imePrezime, size, s._imePrezime);
		_datumRodjenja = new Datum(*s._datumRodjenja);
	}

	~Student() { delete[] _imePrezime; delete _datumRodjenja; }

	Student& operator= (const Student& s)
	{
		if (this != &s)
		{
			delete[] _imePrezime;
			int size = strlen(s._imePrezime) + 1;
			_imePrezime = new char[size];
			strcpy_s(_imePrezime, size, s._imePrezime);

			*_datumRodjenja = *s._datumRodjenja;
		}

		return *this;
	}

	friend bool operator==(const Student&, const Student&);
	friend ostream& operator<<(ostream&, const Student&);
};

bool operator==(const Student& s1, const Student& s2)
{
	return strcmp(s1._imePrezime, s2._imePrezime) == 0 && *s1._datumRodjenja == *s2._datumRodjenja;
}

ostream& operator<<(ostream& cout, const Student& s)
{
	cout << "Ime i prezime: " << s._imePrezime << endl;

	return cout;
}

class Ispit {
	Datum _datumOdrzavanja;
	char * _opisIspita;
	Kolekcija<Student, Datum> _prijave;
	Kolekcija<Student, int> _rezultati;
public:
	Ispit(char* opis, Datum& d): _datumOdrzavanja(d)
	{
		int size = strlen(opis) + 1;
		_opisIspita = new char[size];
		strcpy_s(_opisIspita, size, opis);

	}

	Ispit(const Ispit& i): 
		_datumOdrzavanja(i._datumOdrzavanja), _prijave(i._prijave), _rezultati(i._rezultati)
	{
		//_prijave = i._prijave;

		int size = strlen(i._opisIspita) + 1;
		_opisIspita = new char[size];
		strcpy_s(_opisIspita, size, i._opisIspita);
	}

	~Ispit()
	{
		delete[] _opisIspita;
		_opisIspita = nullptr;
	}

	bool DodajPrijavu(Student& s, Datum& d)
	{
		return _prijave.AddElement(s, d);
	
	}

	void DodajRezultat(Student& s, int ocjena)
	{
		for (int i = 0; i < _rezultati.getTrenutno(); i++)
		{
			if (_rezultati[i] == s)
			{
				_rezultati.GetElement2(i) = ocjena;
				return;
			}
		}

		for (int i = 0; i < _prijave.getTrenutno(); i++)
		{
			if (_prijave[i] == s)
			{
				_rezultati.AddElement(s, ocjena);
				return;
			}
		}
	}

	void ZakljuciRezultate()
	{
		int index;
		for (int i = 0; i < _prijave.getTrenutno(); i++)
		{
			index = -1;
			for (int j = 0; j < _rezultati.getTrenutno(); j++)
			{
				if (_prijave[i] == _rezultati[j])
				{
					index = i;
					break;
				}
			}

			if (index == -1)
			{
				_rezultati.AddElement(_prijave[i], 5);
			}
		}
	}

	friend ostream& operator<<(ostream&, Ispit&);
};

ostream& operator<<(ostream& cout, Ispit& i)
{
	cout << "Datum odrzavanja: " << i._datumOdrzavanja << endl;
	cout << "Opis: " << i._opisIspita << endl;

	cout << "Prijave\n" << i._prijave << endl;
	cout << "Rezultati\n" << endl;

	if (i._prijave.getTrenutno() == i._rezultati.getTrenutno())
		cout << i._rezultati << endl;

	/*for (int j = 0; j < i._rezultati.getTrenutno(); j++)
	{
		if(i._rezultati.GetElement2(j) > 5)
		{
			cout << i._rezultati[j];
			cout << i._rezultati.GetElement2(j);
		}
	}*/

	return cout;

}

char *crt = "\n--------------------------------------------------\n";
void main() {

	//REGIONI olaksavaju organizaciju programskog code-a, te ih mozete ukloniti ukoliko vam otezavaju preglednost

#pragma region TestiranjeKolekcije
	Kolekcija<int, double> kolekcija1;
	for (size_t i = 0; i < 50; i++)
		kolekcija1.AddElement(i, i + 0.3);
	cout << kolekcija1 << endl;
	cout << crt;
	Kolekcija<int, double> kolekcija2;
	kolekcija2 = kolekcija1;//operator= za kolekciju
	kolekcija1.RemoveElement(2);//UMANJITI VELICINU NIZA
	cout << kolekcija2 << crt;
	if (kolekcija1.getTrenutno() == kolekcija2.getTrenutno())
		cout << "ISTI BROJ ELEMENATA" << endl;
	Kolekcija<int, double> kolekcija3(kolekcija2);
	cout << kolekcija3 << crt;
#pragma endregion

#pragma region TestiranjeDatuma
	Datum danas(26, 11, 2015);
	Datum sutra(danas);//kons. kopije za Datum
	Datum prekosutra;
	prekosutra = danas;//operator= za Datum
	cout << danas << endl << sutra << endl << prekosutra << crt;
#pragma endregion
//
#pragma region TestiranjeStudenta
	Student denis("Denis Music", Datum(26, 11, 1990));
	Student jasmin("Jasmin Azemovic", Datum(22, 11, 1990));
	Student goran;
	goran = jasmin; //operator= za klasu Student
	Student adel("Adel Handzic", Datum(25, 8, 1990));
	Student marija("Marija Herceg", Datum(15, 6, 1990));
#pragma endregion
//
#pragma region TestiranjeIspita
	Ispit prIII("PRIII::26.11.2015", danas);
	//DodajPrijavu - dodaje novog studenta u listu prijavljenih za ispti. onemoguciti dodavanje dva ista studenta
	if (prIII.DodajPrijavu(denis, danas))
		cout << denis << " DODAT na spisak" << endl;
	if (prIII.DodajPrijavu(jasmin, danas))
		cout << jasmin << " DODAT na spisak" << endl;
	if (prIII.DodajPrijavu(goran, danas))
		cout << goran << " DODAT na spisak" << endl;
	if (prIII.DodajPrijavu(adel, danas))
		cout << adel << " DODAT na spisak" << endl;
	if (prIII.DodajPrijavu(marija, danas))
		cout << marija << " DODAT na spisak" << endl;

	cout << crt << endl;
	/*DodajRezultat - dodaje ocjenu koju je student ostvario na ispitu. 
	za dodavanje ocjene student je prethodno trebao biti prijavljen na ispit
	*/
	prIII.DodajRezultat(goran, 7);
	prIII.DodajRezultat(jasmin, 6);
	prIII.DodajRezultat(adel, 8);
	prIII.DodajRezultat(marija, 9);

	///*Sortiraj - sortira rezultate ispita prema ocjeni*/
	//prIII.Sortiraj();
	///*ZakljuciRezultate - dodaje ocjenu 5 svim studentima koji su prijavili ispit a nije im evidentirana ocjena*/
	prIII.ZakljuciRezultate();
	////rezultati ispita se smiju ispisivati jedino ukoliko su prethodno zakljuceni
	cout << prIII << crt;

	Ispit prIII_2(prIII);
	cout << prIII_2 << crt;
#pragma endregion
}
