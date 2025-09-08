# Start here:
See this cheetsheet: 
https://www.glukhov.org/post/2024/12/ollama-cheatsheet/

# Install or Upgrade Ollama in Linux
To install or upgrade, run this command
```
curl -fsSL https://ollama.com/install.sh | sh
```

Check the version
```
ollama --version
```

# Check status of Ollma
```
http://localhost:11434/
```

# Run ollama server manually
```
ollama serve
```

# Run
Download and run you model locally after the server is running

First time, it will take time until the model is downloaded from Internet
```
ollama run llama3
```

# List running models
```
ollama list
```

# Install a model
Go the website and find the model of your choice, choose the version and size and copy-paste the command on the page. e.g.

```
ollama run deepseek-r1:1.5b
```

# Huggingface

pip install -U "huggingface_hub[cli]"

# Youtube
#See this video:
#https://www.youtube.com/watch?v=i-txsBoTJtI&t=1s

# MacOS
Alternative to ollama.com, use brew to install ollama
```
brew install ollama
curl -fsSL https://ollama.com/install.sh | sh
pip install ollama
```

<!-- To start ollama now and restart at login:
  brew services start ollama
Or, if you don't want/need a background service you can just run:
  /usr/local/opt/ollama/bin/ollama serve
==> Summary
🍺  /usr/local/Cellar/ollama/0.1.32: 7 files, 25.0MB
==> Running `brew cleanup ollama`...
Disable this behaviour by setting HOMEBREW_NO_INSTALL_CLEANUP.
Hide these hints with HOMEBREW_NO_ENV_HINTS (see `man brew`). -->
