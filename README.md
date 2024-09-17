This is a copy of the main file from the repository with colaborator: Sami Ahmed, Adithya Anand, and our mentor Fadhil Kurnia

# distann
Distributed ANN Serving

## Compiling and running the backend

Prerequisite:
- Install cmake and g++ in your machine
- Install drogon in your machine, ensuring we can access `<drogon/drogon.h>`.
  Follow its official documentation to see the instruction based on your OS using this link `https://github.com/drogonframework/drogon/wiki/ENG-02-Installation`

If you can't install drogon on windows: 

- On Windows if you have problems installing drogon in your machine, you can use install and run the server on WSL by following the steps in this website: `https://learn.microsoft.com/en-us/windows/wsl/install` 

- After you have installed WSL you would follow the Install by source in Linux guide in the installation website. 

- You would also want to install git to access your repository
  ```
  sudo apt install git
  ```

- Before cloning repo make sure you have followed all drogon installation steps found here. `https://github.com/drogonframework/drogon/wiki/ENG-02-Installation`

- To clone a repository on git follow the steps here: `https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository#cloning-a-repository`. 
You would need your Github username and create a token (settings -> Developer settings -> Personal Access Tokens -> Tokens -> Generate new token).

- When generating token make sure you toggle 'repo' on the select scopes section to allow you to have acess to the repo as it is private.  

1. Compile the backend
    ## Compiling and running the backend
    1. Go to the `build` directory, please make the directory if not exist.
      ```
      cd build
      ```
    2. From the `build` directory, generate the `Makefile` with `cmake`:
      ```
      cmake ..
      ```
    3. Compile the backend to generate the `backend` binary:
      ```
      make
      ```
      The command above should produce `backend` (or `backend.exe`) in Windows.
    4. Finally, run the `backend`:
      ```
      ./backend 9000 backend
      ```
      You can access the backend in your browser by visiting 
      `http://localhost:9000`.

2. Open the API server to have the backend return images. 
    ```
    cd backend/API
    python3 app.py --port 4500
    ```
    
3. The compiler will generate an executable called `backend` (or `backend.exe` in Windows) inside the build directory. On linux or Mac can test backend by running ./backend 9000 backend in terminal, then go to http://localhost:9000 to access the backend. 

4. To access the frontend: 

    ```
    cd frontend
    python3 -m http.server (for linux and mac)
    ```
    Then go to your browser to access http://localhost:8000/


## Example request-response

List of backend API endpoints:
| Endpoint                       | HTTP Method   | Example Response          |
| --------                       | -------       |  ---                      |
| `/images/<filename>`           | `GET`         | an actual image, if exist |
| `/api/search?prompt=<prompt>`  | `GET`         | See the example below     |

Project structure:
```
backend/
├─ API/                 // api server directory 
├─ build/               // compilation directory, omitted in git.
├─ images/              // dataset of images, the filename is the image ID.
│  ├─ 0000.png
│  ├─ 0001.png
│  ├─ 0002.png
│  ├─ 0003.png
│  ├─ ...
│  ├─ 9999.png
├─ libs/                // the libraries used by this backend.
│  ├─ clip-cpp/         // model that convert text/image into embeddings.
│  ├─ hnswlib/          // an implementation of HNSW: indexing & search.
│  ├─ usearch/          // another implementation of HNSW: indexing & search.
├─ CMakeLists.txt       // cmake file to generate the Makefile in the build/.
├─ main.cc              // the main function of this backend.
├─ README.md
```
Example request:
```
curl "http://localhost:9000/api/search?prompt=cute cats and dogs in a house"
```
Example of a success response:
```
{
    "prompt": "cute cats and dogs in a house",
    "results": [
        {
            "id": 3,
            "url": "http://localhost:9000/images/0003.png",
            "alt": "cute cats in a house"
        },
        {
            "id": 5,
            "url": "http://localhost:9000/images/0005.png",
            "alt": "cute dogs in a house"
        },
        {
            "id": 84,
            "url": "http://localhost:9000/images/0084.png",
            "alt": "cats and dogs"
        },
        {
            "id": 97,
            "url": "http://localhost:9000/images/0097.png",
            "alt": "cute cats and dogs"
        },
        {
            "id": 102,
            "url": "http://localhost:9000/images/0102.png",
            "alt": "cute cats in a tree house"
        },
        {
            "id": 682,
            "url": "http://localhost:9000/images/0682.png",
            "alt": "dogs in their house"
        },
        {
            "id": 983,
            "url": "http://localhost:9000/images/0983.png",
            "alt": "house full of cats"
        },
        {
            "id": 995,
            "url": "http://localhost:9000/images/0995.png",
            "alt": "cute house in the neighborhood"
        }
    ]
}
```
