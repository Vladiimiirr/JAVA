# JAVA
//Получение данных сайта в формате json и обработка данных
//Работа с потоками 

import java.io.IOException;
import java.io.InputStreamReader;
import java.io.LineNumberReader;
import java.net.MalformedURLException;
import java.net.URL;

public class Stock {
    public static void main(String[] args) throws IOException, InterruptedException {
        Rem[] rb = new Rem[8];
        for (int i = 0;i<8; i++){
            rb[i] = new Rem(i);
            rb[i].start();
        }

        Thread.sleep(5000);// Ожидает выполнения потоков, выводит компании, которые получили ответ на запрос от сайта


        for (int t=0; t<Rem.companys.length;t++)
            if (Rem.companys[t]!=null) System.out.println(Rem.companys[t]);

    }
}

class Chels{
    public static String [] priceDev;
    static byte[] bArray;
    static double [] arrPrice = new double[15];
    static int pr=0;
    static void prebor(){
        int s=0;
        byte [] byt;
        priceDev =Rem.priceCom[0].split(":");
        bArray = priceDev[1].getBytes();
        byt =new byte[7];
        while (s <= (bArray.length-1)){
            if(bArray[s]!= 34 && bArray[s]!=32&& s<=8){
                byt[s-2] = bArray[s];
            }
            s++;
        }
        arrPrice [pr] =Double.parseDouble(new String(byt));
        pr++;

    }
}

class Rem extends Thread{
    int a;
    Rem(int a){
        this.a =a;
    }
    String [] company = {"YNDX","AAPL","GOOGL","GM","TSLA","MSFT","AMZN","MA","BAC"};
    static String [] companys = new String[10];
    static String [] price = new String [20];
    static String [] priceCom;

    int i =0;
    static int j=0,g=0;
    @Override
    public void run() {
        super.run();
        try {
            URL url = new URL("https://www.alphavantage.co/query?function=TIME_SERIES_DAILY&symbol="+company[a]+"&apikey=6PTCRPRGSE7UQJ2C");// стандартная частота вызовов API составляет 5 вызовов в минуту и 500 вызовов в день.
            try {
                LineNumberReader reader = new LineNumberReader(new InputStreamReader(url.openStream()));
                String str = reader.readLine();

                while (str != null) {
                    if (i==13|i==20){
                        //System.out.println(company[a]+str);
                        // System.out.println(priceCom[0]);
                        price [g] = str;
                        g++;
                        if (i==13) {
                            priceCom = str.split(",");
                            Chels.prebor();
                            companys[j]=company[a];
                            j++;
                        }
                    }
                    str = reader.readLine();
                    i++;
                }
                reader.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        } catch (MalformedURLException ex) {
            ex.printStackTrace();
        }
    }
}
