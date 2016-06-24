# blog.pytest.org

This is the repository for the pytest blog. Everyone that has something
interesting to share about pytest is invited to contribute posts to the blog.

# Contributions

The pytest blog is a community driven blog that aims to spread information on
all things pytest.

# How to work with the blog locally

## Setting up
* Fork/clone this repository
* Create a virtual environment (you can put it in the env/ folder) and activate it
* Install the requirements.txt: `pip install -r requirements.txt`


## Running the development server
Now you should be able to run the blog and see it in your browser:

* Start the development server and automatic page generation: `make devserver`
* Open `http://localhost:8000`
* When you are finished, stop the server by running `make stopserver`


## How to write a new article
This blog uses [pelican](http://docs.getpelican.com/).


# Publishing

Publishing is done automatically for all commits in the master branch.  Most of
the time, just push your changes and have them deployed. 

It is also possible to deploy directly from your local machine by
running `make publish`.

If you don't want to check out the repository, you can use the Github web interface to add a new file under a subdirectory of "content".