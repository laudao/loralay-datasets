# Summarization datasets with layout information

## Environment Setup 

~~~shell
$ conda create -n di-benchmark python=3.8
$ conda activate summ-datasets 
$ git clone ssh://git@source.recital.ai:2222/research/doc-intelligence/summ-datasets.git
$ cd summ-datasets
$ pip install -r requirements.txt
~~~

## Extract PDF files 

### From ArXiv and PubMed datasets

PDFs are extracted based on IDs contained in the original data files (downloaded from https://github.com/armancohan/long-summarization). 

For ArXiv, raw abstracts are contained in a separate metadatafile (downloaded from https://www.kaggle.com/Cornell-University/arxiv). For PubMed, they are retrieved using the PMC OAI service. 


To extract from ArXiv:
~~~shell
$ python src/extract_from_arxiv.py --input_file path/to/original/data/file \
                                   --metadata_file path/to/metadata/file \
                                   --pdf_output_dir path/to/pdf/output/dir \
                                   --abstract_output_path path/to/abstract/output/    file \
                                   --n_docs <num_docs_to_process> # -1 to process every document
~~~

To extract from PubMed:
~~~shell
$ python src/extract_from_pubmed.py --input_file path/to/original/data/file \
                                    --pdf_output_dir path/to/pdf/output/dir \
                                    --abstract_output_path path/to/abstract/output/file \
                                    --n_docs <num_docs_to_process> # -1 to process every document
~~~

### From HAL

We extract French articles from HAL using the provided API.

~~~shell
$ python src/extract_from_hal.py --lang fr \
                                 --pdf_output_dir path/to/pdf/output/dir \
                                 --abstract_output_path path/to/abstract/output/file \
                                 --n_docs <num_docs_to_process> # -1 to process every document
~~~

### From SciELO and KoreaScience

We extract PDFs from SciELO and KoreaScience by scraping their websites.

To get documents from the collection `<collection>` (`bol|col|ecu|esp|per|prt|pry|scl|ury|ven`) from SciELO:

~~~shell
$ python src/extract_from_scielo.py \
    --start_url "https://search.scielo.org/?q=*&lang=en&count=50&from=1&output=site&sort=&format=summary&page=1&where=&filter%5Bin%5D%5B%5D=<collection>" \
    --collection_prefix <collection> \
    --output_file path/to/output/file \
    --stop_page <num_pages_to_process> # -1 to process every page
~~~

To get documents published in `<year>` (from 2012 to 2021) from KoreaScience:

~~~shell
$ python src/extract_from_korsc.py \
    --start_url "https://koreascience.or.kr/search/advanced.page?pubs=korean&&pubYr=<year>&pageSize=50&pageNo=1" \
    --output_file path/to/output/file \
    --stop_page <num_pages_to_process> # -1 to process every page
~~~

## Convert PDFs to images

~~~shell
$ python src/convert_pdf_to_image.py --input_dir path/to/pdf/dir \
                                     --output_dir path/to/img/dir \
                                     --n_docs <num_docs_to_process> # -1 to process     every document
~~~

## Convert PDFs to HTMLs

~~~shell
$ python src/convert_pdf_to_html.py --input_dir path/to/dir/containing/pdf/folder \
                                    --pdf_folder pdf/folder \
                                    --output_folder html/folder \
                                    --n_docs <num_docs_to_process>  # -1 to process every document
~~~

## Convert HTMLs to txt

~~~shell
$ python src/parse_html.py --html_dir path/to/html/dir \
                           --output_dir path/to/txt/output/dir \
                           --n_docs num_docs_to_process # -1 to process every document
~~~

## Find and remove abstract from text files 

~~~
$ python src/remove_abstract.py --text_dir path/to/txt/dir \
                                --abstract_path path/to/abstract/file \
                                --main_lang en|fr|es|pt|ko \ 
                                --output_text_dir path/to/output/text/dir \
                                --n_docs <num_docs_to_process> # -1 to process every document
~~~

