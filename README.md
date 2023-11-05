# Windows-Venv-Scripts
Collection of scripts and instructions for handling Python virtual environments from a central folder on Windows.

## Why?

Similar to the ~/.virtualenvs folder on Linux, this is a central folder for all your virtual environments. This allows you to easily manage your virtual environments and keep them separate from your projects.

## How?

### Setup

1. Create a folder for your virtual environments. I use `C:\Users\{username}\virtualenvs`.
2. Create a system environment variable called `WORKON_HOME` and set it to the path of your virtual environments folder.
    ```bat
    setx WORKON_HOME "C:\Users\{username}\virtualenvs"
    ```
3. Create a seperate folder for storing the scripts. I use `C:\Users\{username}\scripts`.
4. Add the scripts folder to your system path.
5. There are three scripts that need to be in the scripts folder:
    - `mkvenv.bat`
        ```bat
        @echo off
        setlocal enabledelayedexpansion
        set "venv_name=%~1"
        set "venv_path=%WORKON_HOME%\%venv_name%"
        python -m venv !venv_path!
        endlocal
        ```
    - `rmvenv.bat`
        ```bat
        @echo off
        setlocal enabledelayedexpansion
        set "venv_name=%~1"
        set "venv_path=%WORKON_HOME%\%venv_name%"
        rmdir /s /q "!venv_path!"
        endlocal
        ```
    - `workon.bat`
        ```bat
        @echo off
        set "venv_name=%~1"
        set "venv_path=%WORKON_HOME%\%venv_name%\Scripts\activate"
        call "%venv_path%"
        ```

### Usage

- Create a new virtual environment:
    ```bat
    mkvenv {venv_name}
    ```
- Activate a virtual environment:
    ```bat
    workon {venv_name}
    ```
- Deactivate a virtual environment:
    ```bat
    deactivate
    ```
- Delete a virtual environment:
    ```bat
    rmvenv {venv_name}
    ```

## Notes

- mkvenv.bat will use the default python version. If you want to use a specific version, you may consider using the below script instead. You can see the available versions by running `py -0`
    ```bat
    @echo off
    setlocal enabledelayedexpansion
    set "venv_name=%~1"
    set "python_version=%~2"

    if "%python_version%"=="" (
        echo Please specify the Python version to use.
        exit /b
    )

    set "venv_path=%WORKON_HOME%\%venv_name%"
    py -%python_version% -m venv !venv_path!
    endlocal
    ```

    I created a seperate script, `mkvenv-v.bat`, for this purpose because I don't always want to specify the version.

## Contributing

Pull requests are welcome. If you have any questions feel free to open an issue about it.