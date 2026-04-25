# BLEEEEEEEH :3
<h1>Sample JAVA code</h1>
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;
import java.io.*; // Import pro práci se soubory

// 1. ABSTRAKCE
abstract class Vozidlo {
    private String znacka;
    private int rokVyroby;

    public Vozidlo(String znacka, int rokVyroby) {
        this.znacka = znacka;
        this.rokVyroby = rokVyroby;
    }

    public String getZnacka() { return znacka; }
    public int getRokVyroby() { return rokVyroby; }

    public abstract void vydatZvuk();

    // Metoda pro převod dat na řádek textu pro uložení
    public abstract String naTextovyRadek();

    public void vypisInfo() {
        System.out.println("Značka: " + znacka + ", Rok výroby: " + rokVyroby);
    }
}

// 2. DĚDIČNOST: Auto
class Auto extends Vozidlo {
    private int pocetDveri;

    public Auto(String znacka, int rokVyroby, int pocetDveri) {
        super(znacka, rokVyroby);
        this.pocetDveri = pocetDveri;
    }

    @Override
    public void vydatZvuk() { System.out.println("Auto dělá: Vrrrruuum!"); }

    @Override
    public String naTextovyRadek() {
        return "AUTO;" + getZnacka() + ";" + getRokVyroby() + ";" + pocetDveri;
    }

    @Override
    public void vypisInfo() {
        super.vypisInfo();
        System.out.println("Počet dveří: " + pocetDveri);
    }
}

// 3. DĚDIČNOST: Motocykl
class Motocykl extends Vozidlo {
    private boolean maPostranniVozik;

    public Motocykl(String znacka, int rokVyroby, boolean maPostranniVozik) {
        super(znacka, rokVyroby);
        this.maPostranniVozik = maPostranniVozik;
    }

    @Override
    public void vydatZvuk() { System.out.println("Motocykl dělá: Brm brm brm!"); }

    @Override
    public String naTextovyRadek() {
        return "MOTO;" + getZnacka() + ";" + getRokVyroby() + ";" + maPostranniVozik;
    }

    @Override
    public void vypisInfo() {
        super.vypisInfo();
        System.out.println("Má postranní vozík: " + (maPostranniVozik ? "Ano" : "Ne"));
    }
}

// Hlavní spouštěcí třída
class HlavniAplikace {
    private static final String SOUBOR_DAT = "garaz.txt";

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        // Načteme data ze souboru hned při startu
        List<Vozidlo> garaz = nactiData();
        boolean bezi = true;

        System.out.println("Vítejte v interaktivní garáži s ukládáním!");

        while (bezi) {
            System.out.println("\n--- MENU ---");
            System.out.println("1 - Přidat Auto | 2 - Přidat Motocykl | 3 - Vypsat garáž | 0 - Konec");
            System.out.print("Volba: ");

            int volba = scanner.nextInt();
            scanner.nextLine();

            switch (volba) {
                case 1:
                    System.out.print("Značka: "); String zA = scanner.nextLine();
                    System.out.print("Rok: "); int rA = scanner.nextInt();
                    System.out.print("Dveře: "); int dA = scanner.nextInt();
                    garaz.add(new Auto(zA, rA, dA));
                    ulozData(garaz); // Uložíme po každé změně
                    break;
                case 2:
                    System.out.print("Značka: "); String zM = scanner.nextLine();
                    System.out.print("Rok: "); int rM = scanner.nextInt();
                    System.out.print("Vozík (true/false): "); boolean vM = scanner.nextBoolean();
                    garaz.add(new Motocykl(zM, rM, vM));
                    ulozData(garaz); // Uložíme po každé změně
                    break;
                case 3:
                    for (Vozidlo v : garaz) { v.vypisInfo(); v.vydatZvuk(); }
                    break;
                case 0:
                    bezi = false;
                    break;
            }
        }
        scanner.close();
    }

    // METODA PRO UKLÁDÁNÍ DO SOUBORU
    private static void ulozData(List<Vozidlo> garaz) {
        try (PrintWriter writer = new PrintWriter(new FileWriter(SOUBOR_DAT))) {
            for (Vozidlo v : garaz) {
                writer.println(v.naTextovyRadek());
            }
            System.out.println("... data uložena do " + SOUBOR_DAT);
        } catch (IOException e) {
            System.out.println("Chyba při ukládání: " + e.getMessage());
        }
    }

    // METODA PRO NAČÍTÁNÍ ZE SOUBORU
    private static List<Vozidlo> nactiData() {
        List<Vozidlo> nactenaGaraz = new ArrayList<>();
        File soubor = new File(SOUBOR_DAT);

        if (!soubor.exists()) return nactenaGaraz; // Pokud soubor neexistuje, vrátíme prázdný seznam

        try (BufferedReader reader = new BufferedReader(new FileReader(soubor))) {
            String radek;
            while ((radek = reader.readLine()) != null) {
                String[] casti = radek.split(";");
                String typ = casti[0];
                String znacka = casti[1];
                int rok = Integer.parseInt(casti[2]);

                if (typ.equals("AUTO")) {
                    int dvere = Integer.parseInt(casti[3]);
                    nactenaGaraz.add(new Auto(znacka, rok, dvere));
                } else if (typ.equals("MOTO")) {
                    boolean vozik = Boolean.parseBoolean(casti[3]);
                    nactenaGaraz.add(new Motocykl(znacka, rok, vozik));
                }
            }
            System.out.println("... data byla úspěšně načtena ze souboru.");
        } catch (IOException e) {
            System.out.println("Chyba při načítání: " + e.getMessage());
        }
        return nactenaGaraz;
    }
}


<img width="493" height="495" alt="image" src="https://github.com/user-attachments/assets/9c94445a-9a6a-4a26-9888-06164290f4e6" />
