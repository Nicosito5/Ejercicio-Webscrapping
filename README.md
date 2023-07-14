# Ejercicio-Webscrapping

import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;
import org.jsoup.nodes.Element;
import org.jsoup.select.Elements;

import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;

public class AmazonWebScraper {
    public static void main(String[] args) {
        // URL de la página de resultados de Amazon
        String url = "https://www.amazon.com/s?k=juegos";

        try {
            // Realizar la solicitud HTTP GET a la página de resultados
            Document document = Jsoup.connect(url).get();

            // Obtener los elementos que contienen los productos
            Elements productElements = document.select(".s-result-item");

            // Crear un nuevo archivo para almacenar los resultados
            BufferedWriter writer = new BufferedWriter(new FileWriter("resultados.csv"));

            // Iterar sobre los elementos de los productos y extraer el título y el precio
            for (Element productElement : productElements) {
                String title = productElement.select(".a-text-normal").text();
                String price = productElement.select(".a-offscreen").text();

                // Escribir el título y el precio en el archivo CSV
                writer.write(title + "," + price);
                writer.newLine();
            }

            // Cerrar el escritor del archivo
            writer.close();

            System.out.println("La extracción de datos se completó con éxito.");

        } catch (IOException e) {
            System.err.println("Ocurrió un error durante la extracción de datos: " + e.getMessage());
        }
    }
}
