# Librerias externas #

Repositorio con las librerias externas de uso más comun en los proyectos de tesoreria. La carpeta docker contiene un contenedor capaz de compilar en linux cada una de las librerias.
- Para poder ejecutar cada imagen, se debe correr el comando:

    docker build -f docker/<docker_filename> -t <tagname> .

Ejemplos:

    docker build -f docker/Dockerfile-Boost -t boost .
    docker build -f docker/Dockerfile-QuantLib -t quantlib .
    docker build -f docker/Dockerfile-QuantExt -t quantext .
    docker build -f docker/Dockerfile-json -t json .
    docker build -f docker/Dockerfile-pybind11 -t pybind11 .
    docker build -f docker/Dockerfile-externalpackages -t externalpackages .


***

## Tips ##

- Boost se descarga de internet, por lo que no es necesario descargarlo previamente.
- Para componer imagenes (ej: usar json con quantlib instalado) es necesario copiar en la nueva imagen del proyecto las carpetas con las librerias. Ej:
  
    FROM quantlib:lastest
    FROM json:lastest
    COPY --from=0 /usr/local/ /usr/local/ # copia todo lo que esta en la imagen 0 (quantlib) en la imagen actual, sin borrar lo de la segunda imagen (json)

- Para compilar en windows, se debe agregar el tag -DBoost_INCLUDE_DIRS, y luego se debe compilar desde Visual Studio. Además, para que funcione find_package de CMAKE, es necesario que el nombre del paquete coicida con el nombre de la carpeta donde se encuentra guardada. Ej (desde la carpeta Engine/QuantLib/build):
  
  - JSON:
  
  cd C:\Users\bloomberg\Desktop\Desarrollo\bankingItau\libs\externalpackages\json\build
  cmake .. -DCMAKE_CXX_STANDARD=20 
  cmake --build . --target INSTALL --config Release -j 16

  - QuantLib:
  
  cd C:\Users\bloomberg\Desktop\Desarrollo\bankingItau\libs\externalpackages\Engine\QuantLib\build
  cmake .. -DCMAKE_CXX_STANDARD=20 -DQL_BUILD_BENCHMARK=OFF -DQL_BUILD_EXAMPLES=OFF -DQL_BUILD_TEST_SUITE=OFF
  cmake --build . --target INSTALL --config Release -j 16

  - QuantExt:
  
  cd C:\Users\bloomberg\Desktop\Desarrollo\bankingItau\libs\externalpackages\Engine\QuantExt\build
  cmake .. -DCMAKE_CXX_STANDARD=20
  cmake --build . --target INSTALL --config Release

  - Json-schema-validator:
   
  cd C:\Users\bloomberg\Desktop\Desarrollo\bankingItau\libs\externalpackages\json-schema-validator\build
  cmake .. -DCMAKE_CXX_STANDARD=20 
  cmake --build . --target INSTALL --config Releasecd

  - Google test:
  
  cd C:\Users\bloomberg\Desktop\Desarrollo\bankingItau\libs\externalpackages\googletest\build
  cmake .. -DCMAKE_CXX_STANDARD=20
  cmake --build . --target INSTALL --config Release

  - pybind11:
  
  cd C:\Users\bloomberg\Desktop\Desarrollo\bankingItau\libs\externalpackages\pybind11\build
  cmake .. -DCMAKE_CXX_STANDARD=20
  cmake --build . --target INSTALL --config Release

  - pybind11_json:
  
  cd C:\Users\bloomberg\Desktop\Desarrollo\bankingItau\libs\externalpackages\googletest\build
  cmake .. -DCMAKE_CXX_STANDARD=20
  cmake --build . --target INSTALL --config Release
  

Para compilar en MACOS/Apple silicon se deben agregar los flags:

    cmake .. -DCMAKE_OSX_ARCHITECTURES=arm64 -DCMAKE_SYSTEM_PROCESSOR=arm64 -DCMAKE_CXX_STANDARD=20
    cmake --build . --target install --config Release -j 12
    cmake --build . --target install --config Debug -j 12