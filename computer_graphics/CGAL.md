# Installation
- [official instllation instruction](https://doc.cgal.org/5.6.2/Manual/usage.html#title)

Follow the following steps to install `CGAL` from source
- CGAL depends on `qt5` and `mpfr` libraries. Install them with
    ```
    sudo apt install libmpfr-dev qt5-default
    ```
- Download `CGAL-<version>-tar.xz` from the [release page](https://github.com/CGAL/cgal/releases).
- Unzip 
    ```
    tar xf CGAL-5.6.2.tar.xz --directory="<target-directory>"
    ```
- cd to <target-directory>
- install 
    ```
    mkdir build
    cmake ..
    sudo make install
    ```

# Viewer
Hit `h` key to see help manual.