
# 🕷️ ScrapeGraphAI: You Only Scrape Once
[![Downloads](https://static.pepy.tech/badge/scrapegraphai)](https://pepy.tech/project/scrapegraphai)
[![linting: pylint](https://img.shields.io/badge/linting-pylint-yellowgreen)](https://github.com/pylint-dev/pylint)
[![Pylint](https://github.com/VinciGit00/Scrapegraph-ai/actions/workflows/pylint.yml/badge.svg)](https://github.com/VinciGit00/Scrapegraph-ai/actions/workflows/pylint.yml)
[![CodeQL](https://github.com/VinciGit00/Scrapegraph-ai/actions/workflows/codeql.yml/badge.svg)](https://github.com/VinciGit00/Scrapegraph-ai/actions/workflows/codeql.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![](https://dcbadge.vercel.app/api/server/gkxQDAjfeX)](https://discord.gg/gkxQDAjfeX)


ScrapeGraphAI is a *web scraping* python library that uses LLM and direct graph logic to create scraping pipelines for websites, documents and XML files.
Just say which information you want to extract and the library will do it for you!

<p align="center">
  <img src="https://raw.githubusercontent.com/VinciGit00/Scrapegraph-ai/main/docs/assets/scrapegraphai_logo.png" alt="Scrapegraph-ai Logo" style="width: 50%;">
</p>


## 🚀 Quick install

The reference page for Scrapegraph-ai is available on the official page of pypy: [pypi](https://pypi.org/project/scrapegraphai/).

```bash
pip install scrapegraphai
```
you will also need to install Playwright for javascript-based scraping:
```bash
playwright install
```

**Note**: it is recommended to install the library in a virtual environment to avoid conflicts with other libraries 🐱

## 🔍 Demo
Official streamlit demo:

[![My Skills](https://skillicons.dev/icons?i=react)](https://scrapegraph-ai-demo.streamlit.app/)

Try it directly on the web using Google Colab:

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1sEZBonBMGP44CtO6GQTwAlL0BGJXjtfd?usp=sharing)

Follow the procedure on the following link to setup your OpenAI API key: [link](https://scrapegraph-ai.readthedocs.io/en/latest/index.html).

## 📖 Documentation

The documentation for ScrapeGraphAI can be found [here](https://scrapegraph-ai.readthedocs.io/en/latest/).

Check out also the docusaurus [documentation](https://scrapegraph-doc.onrender.com/).

## 💻 Usage
You can use the `SmartScraper` class to extract information from a website using a prompt.

The `SmartScraper` class is a direct graph implementation that uses the most common nodes present in a web scraping pipeline. For more information, please see the [documentation](https://scrapegraph-ai.readthedocs.io/en/latest/).
### Case 1: Extracting information using Ollama
Remember to download the model on Ollama separately!

```python
from scrapegraphai.graphs import SmartScraperGraph

graph_config = {
    "llm": {
        "model": "ollama/mistral",
        "temperature": 0,
        "format": "json",  # Ollama needs the format to be specified explicitly
        "base_url": "http://localhost:11434",  # set Ollama URL
    },
    "embeddings": {
        "model": "ollama/nomic-embed-text",
        "base_url": "http://localhost:11434",  # set Ollama URL
    }
}

smart_scraper_graph = SmartScraperGraph(
    prompt="List me all the articles",
    # also accepts a string with the already downloaded HTML code
    source="https://perinim.github.io/projects",
    config=graph_config
)

result = smart_scraper_graph.run()
print(result)

```

### Case 2: Extracting information using Docker

Note: before using the local model remember to create the docker container!
```text
    docker-compose up -d
    docker exec -it ollama ollama pull stablelm-zephyr
```
You can use which models avaiable on Ollama or your own model instead of stablelm-zephyr
```python
from scrapegraphai.graphs import SmartScraperGraph

graph_config = {
    "llm": {
        "model": "ollama/mistral",
        "temperature": 0,
        "format": "json",  # Ollama needs the format to be specified explicitly
        # "model_tokens": 2000, # set context length arbitrarily
    },
}

smart_scraper_graph = SmartScraperGraph(
    prompt="List me all the articles",
    # also accepts a string with the already downloaded HTML code
    source="https://perinim.github.io/projects",  
    config=graph_config
)

result = smart_scraper_graph.run()
print(result)
```


### Case 3: Extracting information using Openai model
```python
from scrapegraphai.graphs import SmartScraperGraph
OPENAI_API_KEY = "YOUR_API_KEY"

graph_config = {
    "llm": {
        "api_key": OPENAI_API_KEY,
        "model": "gpt-3.5-turbo",
    },
}

smart_scraper_graph = SmartScraperGraph(
    prompt="List me all the articles",
    # also accepts a string with the already downloaded HTML code
    source="https://perinim.github.io/projects",
    config=graph_config
)

result = smart_scraper_graph.run()
print(result)
```

### Case 4: Extracting information using Groq
```python
from scrapegraphai.graphs import SmartScraperGraph
from scrapegraphai.utils import prettify_exec_info

groq_key = os.getenv("GROQ_APIKEY")

graph_config = {
    "llm": {
        "model": "groq/gemma-7b-it",
        "api_key": groq_key,
        "temperature": 0
    },
    "embeddings": {
        "model": "ollama/nomic-embed-text",
        "temperature": 0,
        "base_url": "http://localhost:11434", 
    },
    "headless": False
}

smart_scraper_graph = SmartScraperGraph(
    prompt="List me all the projects with their description and the author.",
    source="https://perinim.github.io/projects",
    config=graph_config
)

result = smart_scraper_graph.run()
print(result)
```

### Case 5: Extracting information using Gemini 
```python
from scrapegraphai.graphs import SmartScraperGraph
GOOGLE_APIKEY = "YOUR_API_KEY"

# Define the configuration for the graph
graph_config = {
    "llm": {
        "api_key": GOOGLE_APIKEY,
        "model": "gemini-pro",
    },
}

# Create the SmartScraperGraph instance
smart_scraper_graph = SmartScraperGraph(
    prompt="List me all the articles",
    source="https://perinim.github.io/projects",
    config=graph_config
)

result = smart_scraper_graph.run()
print(result)
```

The output for all 3 the cases will be a dictionary with the extracted information, for example:

```bash
{
    'titles': [
        'Rotary Pendulum RL'
        ],
    'descriptions': [
        'Open Source project aimed at controlling a real life rotary pendulum using RL algorithms'
        ]
}
```

## 🤝 Contributing

Feel free to contribute and join our Discord server to discuss with us improvements and give us suggestions!

Please see the [contributing guidelines](https://github.com/VinciGit00/Scrapegraph-ai/blob/main/CONTRIBUTING.md).

[![My Skills](https://skillicons.dev/icons?i=discord)](https://discord.gg/gkxQDAjfeX)
[![My Skills](https://skillicons.dev/icons?i=linkedin)](https://www.linkedin.com/company/scrapegraphai/)
[![My Skills](https://skillicons.dev/icons?i=twitter)](https://twitter.com/scrapegraphai)

## ❤️ Contributors
[![Contributors](https://contrib.rocks/image?repo=VinciGit00/Scrapegraph-ai)](https://github.com/VinciGit00/Scrapegraph-ai/graphs/contributors)

## 🎓 Citations
If you have used our library for research purposes please quote us with the following reference:
```text
  @misc{scrapegraph-ai,
    author = {Marco Perini, Lorenzo Padoan, Marco Vinciguerra},
    title = {Scrapegraph-ai},
    year = {2024},
    url = {https://github.com/VinciGit00/Scrapegraph-ai},
    note = {A Python library for scraping leveraging large language models}
  }
```

## Authors

<p align="center">
  <img src="https://raw.githubusercontent.com/VinciGit00/Scrapegraph-ai/main/docs/assets/logo_authors.png" alt="Authors Logos">
</p>

|                    | Contact Info         |
|--------------------|----------------------|
| Marco Vinciguerra  | [![Linkedin Badge](https://img.shields.io/badge/-Linkedin-blue?style=flat&logo=Linkedin&logoColor=white)](https://www.linkedin.com/in/marco-vinciguerra-7ba365242/)    |
| Marco Perini       | [![Linkedin Badge](https://img.shields.io/badge/-Linkedin-blue?style=flat&logo=Linkedin&logoColor=white)](https://www.linkedin.com/in/perinim/)   |
| Lorenzo Padoan     | [![Linkedin Badge](https://img.shields.io/badge/-Linkedin-blue?style=flat&logo=Linkedin&logoColor=white)](https://www.linkedin.com/in/lorenzo-padoan-4521a2154/)  |

## 📜 License

ScrapeGraphAI is licensed under the MIT License. See the [LICENSE](https://github.com/VinciGit00/Scrapegraph-ai/blob/main/LICENSE) file for more information.

## Acknowledgements

- We would like to thank all the contributors to the project and the open-source community for their support.
- ScrapeGraphAI is meant to be used for data exploration and research purposes only. We are not responsible for any misuse of the library.

## 📈 Roadmap

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta http-equiv="X-UA-Compatible" content="ie=edge">
<title>Markmap</title>
<style>
* {
  margin: 0;
  padding: 0;
}
#mindmap {
  display: block;
  width: 100vw;
  height: 100vh;
}
</style>
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/markmap-toolbar@0.17.0/dist/style.css">
</head>
<body>
<svg id="mindmap"></svg>
<script src="https://cdn.jsdelivr.net/npm/d3@7.8.5/dist/d3.min.js"></script><script src="https://cdn.jsdelivr.net/npm/markmap-view@0.17.0/dist/browser/index.js"></script><script src="https://cdn.jsdelivr.net/npm/markmap-toolbar@0.17.0/dist/index.js"></script><script>(r => {
                setTimeout(r);
              })(() => {
  const {
    markmap,
    mm
  } = window;
  const {
    el
  } = markmap.Toolbar.create(mm);
  el.setAttribute('style', 'position:absolute;bottom:20px;right:20px');
  document.body.append(el);
})</script><script>((getMarkmap, getOptions, root2, jsonOptions) => {
              const markmap = getMarkmap();
              window.mm = markmap.Markmap.create(
                "svg#mindmap",
                (getOptions || markmap.deriveOptions)(jsonOptions),
                root2
              );
            })(() => window.markmap,null,{"content":"<strong>ScrapGraphAI Roadmap</strong>","children":[{"content":"<strong>Short-Term Goals</strong>","children":[{"content":"\n<p data-lines=\"5,6\">Integration with more llm APIs</p>","children":[],"payload":{"lines":"5,7"}},{"content":"\n<p data-lines=\"7,8\">Test proxy rotation implementation</p>","children":[],"payload":{"lines":"7,9"}},{"content":"\n<p data-lines=\"9,10\">Add more search engines inside the SearchInternetNode</p>","children":[],"payload":{"lines":"9,11"}},{"content":"\n<p data-lines=\"11,12\">Improve the documentation (ReadTheDocs)</p>","children":[{"content":"<a href=\"https://github.com/VinciGit00/Scrapegraph-ai/issues/102\">Issue #102</a>","children":[],"payload":{"lines":"12,14"}}],"payload":{"lines":"11,14"}},{"content":"\n<p data-lines=\"14,15\">Create tutorials for the library</p>","children":[],"payload":{"lines":"14,16"}}],"payload":{"lines":"3,4"}},{"content":"<strong>Medium-Term Goals</strong>","children":[{"content":"\n<p data-lines=\"18,19\">Node for handling API requests</p>","children":[],"payload":{"lines":"18,20"}},{"content":"\n<p data-lines=\"20,21\">Improve SearchGraph to look into the first 5 results of the search engine</p>","children":[],"payload":{"lines":"20,22"}},{"content":"\n<p data-lines=\"22,23\">Make scraping more deterministic</p>","children":[{"content":"Create DOM tree of the website","children":[],"payload":{"lines":"23,24"}},{"content":"HTML tag text embeddings with tags metadata","children":[],"payload":{"lines":"24,25"}},{"content":"Study tree forks from root node","children":[],"payload":{"lines":"25,26"}},{"content":"How do we use the tags parameters?","children":[],"payload":{"lines":"26,28"}}],"payload":{"lines":"22,28"}},{"content":"\n<p data-lines=\"28,29\">Create scraping folder with report</p>","children":[{"content":"Folder contains .scrape files, DOM tree files, report","children":[],"payload":{"lines":"29,30"}},{"content":"Report could be a HTML page with scraping speed, costs, LLM info, scraped content and DOM tree visualization","children":[],"payload":{"lines":"30,31"}},{"content":"We can use pyecharts with R-markdown","children":[],"payload":{"lines":"31,33"}}],"payload":{"lines":"28,33"}},{"content":"\n<p data-lines=\"33,34\">Scrape multiple pages of the same website</p>","children":[{"content":"Create new node that instantiate multiple graphs at the same time","children":[],"payload":{"lines":"34,35"}},{"content":"Make graphs run in parallel","children":[],"payload":{"lines":"35,36"}},{"content":"Scrape only relevant URLs from user prompt","children":[],"payload":{"lines":"36,37"}},{"content":"Use the multi dimensional DOM tree of the website for retrieval","children":[],"payload":{"lines":"37,38"}},{"content":"<a href=\"https://github.com/VinciGit00/Scrapegraph-ai/issues/112\">Issue #112</a>","children":[],"payload":{"lines":"38,40"}}],"payload":{"lines":"33,40"}},{"content":"\n<p data-lines=\"40,41\">Crawler graph</p>","children":[{"content":"Scrape all the URLs with the same domain in all the pages","children":[],"payload":{"lines":"41,42"}},{"content":"Build many DOM trees and link them together","children":[],"payload":{"lines":"42,43"}},{"content":"Save the multi dimensional tree in a file","children":[],"payload":{"lines":"43,45"}}],"payload":{"lines":"40,45"}},{"content":"\n<p data-lines=\"45,46\">Compare two DOM trees to assess the similarity</p>","children":[{"content":"Save the DOM tree of the scraped website in a file as a sort of cache to be used to compare with future website structure","children":[],"payload":{"lines":"46,47"}},{"content":"Create similarity metrics with multiple DOM trees (overall tree? only relevant tags structure?)","children":[],"payload":{"lines":"47,49"}}],"payload":{"lines":"45,49"}},{"content":"\n<p data-lines=\"49,50\">Nodes for handling authentication</p>","children":[{"content":"Use Selenium or Playwright to handle authentication","children":[],"payload":{"lines":"50,51"}},{"content":"Passes the cookies to the other nodes","children":[],"payload":{"lines":"51,53"}}],"payload":{"lines":"49,53"}},{"content":"\n<p data-lines=\"53,54\">Nodes that attaches to an open browser</p>","children":[{"content":"Use Selenium or Playwright to attach to an open browser","children":[],"payload":{"lines":"54,55"}},{"content":"Navigate inside the browser and scrape the content","children":[],"payload":{"lines":"55,57"}}],"payload":{"lines":"53,57"}},{"content":"\n<p data-lines=\"57,58\">Nodes for taking screenshots and understanding the page layout</p>","children":[{"content":"Use Selenium or Playwright to take screenshots","children":[],"payload":{"lines":"58,59"}},{"content":"Use LLM to asses if it is a block-like page, paragraph-like page, etc.","children":[],"payload":{"lines":"59,60"}},{"content":"<a href=\"https://github.com/VinciGit00/Scrapegraph-ai/issues/88\">Issue #88</a>","children":[],"payload":{"lines":"60,62"}}],"payload":{"lines":"57,62"}}],"payload":{"lines":"16,17"}},{"content":"<strong>Long-Term Goals</strong>","children":[{"content":"\n<p data-lines=\"64,65\">Automatic generation of scraping pipelines from a given prompt</p>","children":[],"payload":{"lines":"64,66"}},{"content":"\n<p data-lines=\"66,67\">Create API for the library</p>","children":[],"payload":{"lines":"66,68"}},{"content":"\n<p data-lines=\"68,69\">Finetune a LLM for html content</p>","children":[],"payload":{"lines":"68,69"}}],"payload":{"lines":"62,63"}}],"payload":{"lines":"1,2"}},{"colorFreezeLevel":2,"maxWidth":500})</script>
</body>
</html>
