Feel free to use this repository as a [template](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-repository-from-a-template)

You can manually create your project as follows.

## Creating a package

Create a project root directory and `cd` into it

```
$ mkdir MyProject
$ cd !$
```

Create a `README` and `.gitignore` in this root directory

From the root directory:

```
$ mkdir src/application
$ cd !$
$ touch __init__.py app.py
```

Using your favorite editor, ensure __init__.py has a minimum of a project description and version number:

```python
# MyProject/src/application/__init__.py
"""Project Description"""
__version__ = "0.1.0"
```

Your main python script should have the __main__ wrapper so you can use your code as a module. A main() function can also be your entry point for calling the script (as seen later):

```python
# MyProject/src/application/app.py
def main():
    pass

if __name__ == "__main__":
    main()
```

In your `application` directory, you can also add static files and a configuration file if needed. These will be included automatically when you go to package your file.

Go back to the root directory and create a python virtual environment:

```bash
$ python -m venv .venv
$ source .venv/bin/activate
```

Install the `flit` package which will be used to package your project. :

```bash
$ pip install flit
$ flit init
```

Running `flit init` will automatically populate your project with a `pyproject.toml` and `LICENSE` (follow the on screen prompts to fill out information).

### Project Structure

```
 MyProject
├──  .venv
├──  src
│  └──  application
│     ├──  data_files
│     │  └──  static_file
│     ├──  __init__.py
│     ├──  app.py
│     └──  config.ini
├──  .gitignore
├──  LICENSE
├──  pyproject.toml
└──  README.md
```

### pyproject.toml

You will need to edit the `pyproject.toml` to add your project's dependencies and entry point.

Under `[project]`, add your dependencies as such:
```
[project]
.
.
dependencies = [
    "package1",
    "package2",
]
```

To create the command the user will run to run your program, create an entry point:

```
[project.scripts]
command = "application.app:main"
```

where `application` is the module name, `app` is the script that will be run, and `main` is the function that exists in the app script. Typically your command should be the same name as your application so as to avoid confusion.

### Uploading to PyPi
Follow instructions in the [flit docs](https://flit.readthedocs.io/en/latest/upload.html) to learn how to upload your package to `PyPi` so your package is installable by other users using `pip`

You will need to register an account on both [TestPyPi](https://test.pypi.org/) and [PyPi](https://pypi.org/) (two separate accounts) 

In account settings, generate an `API token`. Create a `.pypirc` file in your system's home directory and populate it (note the username should be the keyword `__token__`), and the password is the generated api token:

```
# ~/.pypirc
[distutils]
index-servers =
   pypi
   testpypi

[pypi]
repository = https://upload.pypi.org/legacy/
username = yourname

[testpypi]
repository = https://test.pypi.org/legacy/
username = __token__
password = pypi-Someverylongtokenthatwasgenerated
```

You are now ready to test your package (assuming your code does something useful ;) ). In your project root, run

```
$ flit publish --repository testpypi
```

You can test downloading your package in another location using 

```
$ pip install -i https://test.pypi.org/simple/ application
```

If all runs well, then you can simply publish to the pypi repository instead.