# ICSDClient
A python interface for accessing the ICSD API Client with the requests library. Please visit the [Fitz-Karlsruhe website](https://icsd.fiz-karlsruhe.de/index.xhtml) for further details on accessing the API. 

## Basic Usage 

First instantiate a client object with the username and password provided by Fitz-Karlsruhe

```python
client = ICSDClient("YOUR_USERNAME", "YOUR_PASSWORD")
```

Once this has authenticated successfully you can use this client to poll the ICSD and retrieve cif files. 

```python
cif_file = client.fetch_cif(1)
cif_files = client.fetch_cifs([1, 2, 3])
```

A search of the ICSD can be performed and which will return the resultant ICSD IDs, with their associated compositions

```python
search = client.search("LiCl")
```

Once a search has been performed these can be passed to `fetch_cifs()` for bulk download.

```python
cifs = client.fetch_cifs(search)
```

These can be written to `.cif` files using the `writeout()` method. These will be saved to the `./cifs/` folder by default, but this can be changed via the `folder` parameter.

```python
client.writeout(cifs)
client.writeout(cifs, folder="/YOUR/STORAGE/PATH")
```

More advanced searches can be performed with a search dictionary. All available search fields can be viewed with `client.search_dict.keys()`. The default search type is AND however this can be changed to OR with `advanced_search(search_type="or")`. 

```python
search_dict = {"authors": "Rosseinsky",
               "composition" : "O",
               "numberofelements": 3}

search = client.advanced_search(search_dict)
cifs = client.fetch_cifs(search)
```

Try to ensure that you log out correctly at the end of the session by calling `client.logout()`. If you are not successfully logged out you will need to wait an hour for the authorization token to expire.

A session history of all server responses can be found in `client.session_history`, make sure to save any large searches.
