# Analisis-y-dise-o-de-algoritmos
Tareas



import java.io.*;
import java.util.*;

public class OrdenacionBasica {

    public static void main(String[] args) throws IOException {
        String archivo = "datos.txt";
        String archivoOrdenado = "ordenado.txt";

        BufferedReader br = new BufferedReader(new FileReader(archivo));
        List<String> archivosTemp = new ArrayList<>();
        List<Integer> bloque = new ArrayList<>();
        String linea;
        int contador = 0;

        while ((linea = br.readLine()) != null) {
            String[] numeros = linea.split(" ");
            for (String n : numeros) {
                if (!n.equals("")) {
                    bloque.add(Integer.parseInt(n));
                    if (bloque.size() == 5) {
                        Collections.sort(bloque);
                        String nombreTemp = "temp" + contador + ".txt";
                        guardarBloque(bloque, nombreTemp);
                        archivosTemp.add(nombreTemp);
                        bloque.clear();
                        contador++;
                    }
                }
            }
        }
        if (!bloque.isEmpty()) {
            Collections.sort(bloque);
            String nombreTemp = "temp" + contador + ".txt";
            guardarBloque(bloque, nombreTemp);
            archivosTemp.add(nombreTemp);
        }
        br.close();

        mezclarArchivos(archivosTemp, archivoOrdenado);

        System.out.println("Archivo ordenado creado: " + archivoOrdenado);
    }

    static void guardarBloque(List<Integer> bloque, String nombreArchivo) throws IOException {
        PrintWriter pw = new PrintWriter(new FileWriter(nombreArchivo));
        for (int n : bloque) {
            pw.print(n + " ");
        }
        pw.close();
    }

    static void mezclarArchivos(List<String> archivos, String archivoFinal) throws IOException {
        PriorityQueue<Elemento> cola = new PriorityQueue<>();
        List<BufferedReader> lectores = new ArrayList<>();

        for (int i = 0; i < archivos.size(); i++) {
            BufferedReader br = new BufferedReader(new FileReader(archivos.get(i)));
            lectores.add(br);
            String linea = br.readLine();
            if (linea != null && !linea.equals("")) {
                String[] nums = linea.split(" ");
                cola.add(new Elemento(Integer.parseInt(nums[0]), i, 0, nums));
            }
        }

        PrintWriter pw = new PrintWriter(new FileWriter(archivoFinal));

        while (!cola.isEmpty()) {
            Elemento e = cola.poll();
            pw.print(e.valor + " ");

            int siguiente = e.indiceArray + 1;

            if (siguiente < e.numeros.length) {
                cola.add(new Elemento(Integer.parseInt(e.numeros[siguiente]), e.indiceArchivo, siguiente, e.numeros));
            } else {
                String linea = lectores.get(e.indiceArchivo).readLine();
                if (linea != null && !linea.equals("")) {
                    String[] nums = linea.split(" ");
                    cola.add(new Elemento(Integer.parseInt(nums[0]), e.indiceArchivo, 0, nums));
                }
            }
        }

        for (BufferedReader br : lectores) br.close();
        pw.close();
        for (String f : archivos) {
            File temp = new File(f);
            if (temp.exists()) {
                temp.delete();
            }
        }
    }

    static class Elemento implements Comparable<Elemento> {
        int valor, indiceArchivo, indiceArray;
        String[] numeros;

        Elemento(int valor, int archivo, int indiceArray, String[] numeros) {
            this.valor = valor;
            this.indiceArchivo = archivo;
            this.indiceArray = indiceArray;
            this.numeros = numeros;
        }

        public int compareTo(Elemento otro) {
            if (this.valor < otro.valor) {
                return -1;
            } else if (this.valor > otro.valor) {
                return 1;
            } else {
                return 0;
            }
        }
    }
}
