# Arabic-to-SQL

Arabic text preprocessing
1.	Unifying Different Forms of Alef (ا): Arabic contains different forms of the letter Alef, including:
o	إ → ا
o	أ → ا
o	آ → ا 
2.	Replacing Final 'ى' with 'ي': The Arabic letter 'ى' (without dots) is replaced with 'ي' to standardize words.
3.	Replacing 'ة' with 'ه': The character 'ة' (Taa Marbouta) is replaced with 'ه' to avoid ambiguity in text processing.
4.	Removing Kashida (ـ): Kashida is used in Arabic script for elongation in writing but has no effect on meaning, so it is removed.
5.	Removing Punctuation Marks (e.g., '?' and '.'): Certain punctuation marks are removed to maintain a clean text representation.
6.	Diacritics Removal: The function dediac_ar(text) is used to remove diacritics (Tashkeel), which are often unnecessary for NLP tasks and can introduce noise.
7.	Applies Arabic reshaping to ensure that characters are displayed correctly.
Retrieval-Augmented Generation (RAG)
First the pre-trained BERT is used for embedding and generates vector embeddings for textual data. Then a FAISS index is built to facilitate fast and efficient retrieval of relevant texts. The process involves:
1.	Encoding textual data into numerical embeddings using the BERT.
2.	Converting the embeddings into a format suitable for FAISS indexing.
3.	Initializing a FAISS index that supports nearest-neighbor searches based on L2 distance (Euclidean distance).
Multiple FAISS indices was applied , where each database is identified by a unique db_id. 
Retrieving Relevant Texts
To retrieve relevant texts for a given query, the system follows these steps:
1.	Identifies the appropriate FAISS index and stored texts based on the given db_id.
2.	Converts the query into an embedding vector using the transformer model.
3.	Searches the FAISS index to find the closest embeddings using L2 distance.
4.	Retrieves and returns the top three relevant text(s) based on the nearest matches

Data preprocessing 
To prepare the data for training, the preprocessing pipeline involves:
1.	Extracting the Arabic question, database ID, and query from the input example.
2.	Retrieving relevant schema context based on the question and database ID.
3.	Constructing an input text that combines the question and retrieved context.
4.	Tokenizing both the input text and the expected query output using a tokenizer with padding and truncation.
5.	Adjusting the label token IDs to replace padding tokens with a special value to ensure proper loss calculation during training.

Formatted Model Input:
عدد رؤساء الأقسام الذين تزيد أعمارهم عن 56. Context: Table: management, Columns: department_ID, head_ID, temporary_acting. Table: department, Columns: Department_ID, Name, Creation, Ranking, Budget_in_Billions, Num_Employees. Table: head, Columns: head_ID, name, born_state, age

AraBART
AraBART is chosen for this task because it is specifically designed for Arabic text generation and transformation. It leverages a transformer-based architecture optimized for Arabic linguistic structures, making it well-suited for tasks such as text summarization, question answering, and SQL generation.

AraBART Architecture:
•	AraBART is based on the BART (Bidirectional and Auto-Regressive Transformers) architecture, which consists of an encoder-decoder framework.
•	The encoder processes input sequences bidirectionally, capturing complex contextual dependencies.
•	The decoder generates output sequences autoregressively, making it effective for text generation tasks.
•	The model is pre-trained using denoising objectives, such as text corruption and sentence permutation, which improve its ability to reconstruct meaningful text sequences.
By fine-tuning AraBART for 65 epochs, the model is further specialized for text-to-SQL generation, allowing it to generate structured SQL queries from Arabic natural language input with higher results.

Model Evaluation and Results
The final trained model was evaluated based on loss values and performance metrics:
•	Final Training Loss: 0.011000
•	Final Validation Loss: 0.447823
The model was tested on 20 questions across multiple databases to ensure robustness. The evaluation metrics were:
•	Average BLEU Score: 0.8634535319062493
•	Exact Match Accuracy: 0.6
•	Execution Accuracy: 0.7
