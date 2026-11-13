✅ Naprawa kodowania CSV krok po kroku

1️⃣ Wczytaj ten plik bez podawania kodowania (czyli tak, jak jest):

df = pd.read_csv(DATA_CSV, encoding="utf-8", errors="replace")


2️⃣ Wyświetl próbkę, żeby potwierdzić, że litery są „zepsute” (już są, więc OK).

3️⃣ Zapisz go ponownie w czystym UTF-8, który Jupyter, VS Code i Python zawsze odczytają poprawnie:

CLEAN_PATH = DATA_CSV.parent / "plants_clean_final_utf8.csv"
df.to_csv(CLEAN_PATH, index=False, encoding="utf-8")
print("💾 Zapisano poprawną wersję:", CLEAN_PATH)


4️⃣ Wczytaj tę wersję ponownie, by potwierdzić:

df = pd.read_csv(CLEAN_PATH, encoding="utf-8")
df.loc[:2, ["tempmin_pasmo", "tempmax_pasmo", "category_group", "light_level_clean"]]


💡 Po co to robimy:

usuwa raz na zawsze problem z Ĺ›, Ä…, itp.;

UTF-8 jest uniwersalny (czytelny w Linux, VS Code, Jupyter, GitHub);

w przyszłych etapach (PCA, k-means, rekomendacje) nie będzie już żadnych problemów z dekodowaniem.

Jeśli po tym kroku teksty będą już wyglądać jak:

Lubi ciepło
Normalna tolerancja ciepła
Wiszaca
Rozproszone światło


to ten plik plants_clean_final_utf8.csv powinniśmy uznać za nową, właściwą wersję i podmienić go w projekcie.

