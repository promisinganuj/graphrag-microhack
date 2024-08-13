# Microsoft GraphRAG Microhack

## Software prerequisites

- [Docker](https://www.docker.com/)
- [VSCode](https://code.visualstudio.com/)
- [Visual Studio Code Remote Development Extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack)

It is strongly recommended to use dev container to avoid python packages dependencies.

## Setting up workspace and running the indexing process

If you want to run the indexing process on your local machine to gain a better understanding of how GraphRAG works, you can follow the steps belows. Please note that these steps are well described in the original documentation at https://microsoft.github.io/graphrag/posts/get_started/. However, we have provided a brief overview of the steps below.

If you don't want to run the indexing process, please move to the next section to [understand the output](#understanding-the-output).

### Steps

1. Clone the repo to your local machine (or Download as zip and extract):

   ```bash
   git clone https://github.com/promisinganuj/graphrag-microhack.git
   ```

1. Open the project inside the vscode dev container. For that open the command palette (`Ctrl+Shift+P`) and search for `Dev Containers: Open Folder in Container...`. Select the root folder and confirm.

2. Choose a root directory called `sample` (or anything else), and create an `input` folder within it.

   ```bash
   mkdir -p ./sample/input
   ```

   This will install the required packages for running the indexing process.

3. For running out the indexation process, you can use any dataset of your choice. We have provided the following dataset in this repo:
   - [datasets/books/a-christmas-carol.txt](./datasets/books/a-christmas-carol.txt) - A copy of "A Christmas Carol" by Charles Dickens from [Project Gutenburg](https://www.gutenberg.org/cache/epub/24022/pg24022.txt).

   Copy the `a-christmas-carol.txt` file to the `./sample` directory by using the following command:

   ```bash
   cp ./datasets/books/a-christmas-carol.txt ./sample/input/
   ```

4. Initialize graphrag by running the following command:

   ```bash
   python -m graphrag.index --init --root ./sample
   ```

5. Setup workspace variables

   This will create two files: .env and settings.yaml in the ./sample directory.

   The `.env` file contains the environment variables required to run the GraphRAG pipeline. If you inspect the file, you'll see a single environment variable defined, `GRAPHRAG_API_KEY=<API_KEY>`. This is the API key for the OpenAI API or Azure OpenAI endpoint. You can replace this with your own API key.

   The `settings.yaml` file contains the settings for the pipeline. You can modify this file to change the settings for the pipeline.

   __OpenAI and Azure OpenAI__
   To run in OpenAI mode, just make sure to update the value of `GRAPHRAG_API_KEY` in the `.env` file with your OpenAI API key.

   __Azure OpenAI__
   In addition, Azure OpenAI users should set the following variables in the `settings.yaml` file. To find the appropriate sections, just search for the `llm: configuration`, you should see two sections, one for the chat endpoint and one for the embeddings endpoint. Here is an example of how to configure the chat endpoint:

   ```yaml
   type: azure_openai_chat # Or azure_openai_embedding for embeddings
   model: <azure_model_name>
   api_base: https://<instance>.openai.azure.com
   api_version: 2024-02-15-preview # You can customize this for other versions
   deployment_name: <azure_model_deployment_name>
   ```

   Please follow the [documentation](https://learn.microsoft.com/en-us/azure/ai-services/openai/how-to/create-resource?pivots=web-portal) for more information on how to set up the Azure OpenAI Service resource and deploy models.

6. Run the Indexing pipeline

   ```bash
   python -m graphrag.index --root ./sample
   ```

   This process will take some time to run. This depends on the size of your input data, what model you're using, and the text chunk size being used (these can be configured in your `.env` file). Once the pipeline is complete, you should see a new folder called `./sample/output/<timestamp>/artifacts` with a series of parquet files.

   Here are the screenshots of the output of a sample run:

   ![indexing-process-output-01](./images/sample-run-01.png)
   ![indexing-process-output-02](./images/sample-run-02.png)
   ![indexing-process-output-03](./images/sample-run-03.png)
   ![indexing-process-output-04](./images/sample-run-04.png)
   ![indexing-process-output-05](./images/sample-run-05.png)
   ![indexing-process-output-06](./images/sample-run-06.png)
   ![indexing-process-output-07](./images/sample-run-07.png)

## Understanding the output

### Understanding the output files

The output of the indexing process is stored in the `./sample/output/<timestamp>/artifacts` directory. For reference, we have provided a sample output in the [sample-output](./sample-output/output/20240812-141408/artifacts/) directory. Here is a brief overview of the output files:

```bash
create_base_text_units.parquet              -
create_base_extracted_entities.parquet      -
create_summarized_entities.parquet          -
create_base_entity_graph.parquet            -
create_final_entities.parquet               -
create_final_nodes.parquet                  -
create_final_communities.parquet            -
join_text_units_to_entity_ids.parquet       -
create_final_relationships.parquet          -
join_text_units_to_relationship_ids.parquet -
create_final_community_reports.parquet      -
create_final_text_units.parquet             -
create_base_documents.parquet               -
create_final_documents.parquet              -
stats.json                                  -
```

### Visualizing the Graph

Please follow the [visualizing-graphrag-output.ipynb](./notebooks/visualizing-graphrag-output.ipynb) notebook to visualize the graph. The notebook provides step-by-step instructions and code cells to:

- Load and pre-process the text data.
- Reshape and load the knowledge graph data for visualization.
- Run graph enabled RAG queries against the knowledge graph
- Visualize the retrieved data as a graph

The notebook requires certain environment variables to be set. For that, please rename the `.env.sample` file in the [notebooks](./notebooks/) folder to `.env` and update the values of the following environment variables:

```bash
GRAPHRAG_API_KEY=
GRAPHRAG_API_BASE=https://<instance>.openai.azure.com/
GRAPHRAG_API_VERSION=2024-05-01-preview
GRAPHRAG_LLM_MODEL=
GRAPHRAG_LLM_DEPLOYMENT=
GRAPHRAG_EMBEDDING_MODEL=
GRAPHRAG_EMBEDDING_DEPLOYMENT=
```

## References:

- [Project GraphRAG](https://www.microsoft.com/en-us/research/project/graphrag/)
- [Microsoft GraphRAG Documentation](https://microsoft.github.io/graphrag/)
- [Microsoft GraphRAG Repo](https://github.com/microsoft/graphrag)
- [Microsoft GraphRAG Accelerator](https://github.com/Azure-Samples/graphrag-accelerator)

## Acknowledgements

- The developers of the Python libraries used for graph creation and visualization.
- The developers of the Microsoft's `graphrag` library.

## License

This project is licensed under the MIT License - see the [LICENSE](./LICENSE) file for details.
