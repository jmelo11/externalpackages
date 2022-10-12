# Librerias externas #

Repositorio con las librerias externas de uso más comun en los proyectos de tesoreria. La carpeta docker contiene un contenedor capaz de compilar en linux cada una de las librerias.
- Para poder ejecutar cada imagen, se debe correr el comando:

    docker build -f docker/<docker_filename> -t <tagname> .

Ejemplos:

    docker build -f docker/Dockerfile-Boost -t boost .
    docker build -f docker/Dockerfile-QuantLib -t quantlib .
    docker build -f docker/Dockerfile-QuantExt -t quantext .
    docker build -f docker/Dockerfile-json -t json .


***

## Tips ##

- Boost se descarga de internet, por lo que no es necesario descargarlo previamente.
- Para componer imagenes (ej: usar json con quantlib instalado) es necesario copiar en la nueva imagen del proyecto las carpetas con las librerias. Ej:
  
    FROM quantlib:lastest
    FROM json:lastest
    COPY --from=0 /usr/local/ /usr/local/ # copia todo lo que esta en la imagen 0 (quantlib) en la imagen actual, sin borrar lo de la segunda imagen (json)

- Para compilar en windows, se debe agregar el tag -DBoost_INCLUDE_DIRS, y luego se debe compilar desde Visual Studio. Ej (desde la carpeta Engine/QuantLib/build):
  
  - QuantLib:

        cmake .. -DCMAKE_CXX_STANDARD=20 -DBoost_INCLUDE_DIR='C:\Users\bloomberg\Desktop\Desarrollo\boost' -DCMAKE_INSTALL_PREFIX='C:\Users\bloomberg\Desktop\Desarrollo\bankingItau\libs\externalpackages\builds\QuantLib'
        cmake --build . --target INSTALL --config Release

  - Json-schema-validator:

        cmake .. -DCMAKE_CXX_STANDARD=20 -DCMAKE_INSTALL_PREFIX='C:\Users\bloomberg\Desktop\Desarrollo\bankingItau\libs\externalpackages\builds\json-schema-validator'    -Dnlohmann_json_DIR='C:\Users\bloomberg\Desktop\Desarrollo\bankingItau\libs\externalpackages\builds\json'
        cmake --build . --target INSTALL --config Release

