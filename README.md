# Analisis-y-dise-o-de-algoritmos
Tareas




import java.io.*;
import java.util.*;

public class OrdenacionExternaSimple {
    public static void main(String[] args) throws IOException {
        // 1. Creamos un archivo con datos desordenados
        String archivo = "datos.txt";
        try (PrintWriter pw = new PrintWriter(new FileWriter(archivo))) {
            pw.println("8 3 7 1 9");
            pw.println("4 2 6 5");
        }

        // 2. Leemos el archivo y ordenamos cada bloque
        List<Integer> todos = new ArrayList<>();
        try (BufferedReader br = new BufferedReader(new FileReader(archivo))) {
            String linea;
            while ((linea = br.readLine()) != null) {
                for (String num : linea.split(" ")) {
                    todos.add(Integer.parseInt(num));
                }
            }
        }

        // 3. Ordenamos todos los datos
        Collections.sort(todos);

        // 4. Mostramos el resultado
        System.out.println("Resultado ordenado: " + todos);
    }
}
