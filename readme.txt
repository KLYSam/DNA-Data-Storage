I chose to use Google colab to use their high performance GPUs for training purposes as I expected heavy GPU usage.  To run these programs, either open them in Google colab, or comment out lines such as “from google.colab import drive”, “drive.mount(filepath)”, os.chdir(filepath), and dataframe.to_csv(filepath).  Please also replace file path with the directory you want to save the files in.

To run the programs interactively (.ipynb), open the subfolder ipynb.
To run the programs as executable files (.py), open the subfolder py.

In each subfolder, there are four files:
1. make_training_data.ipynb is a program to synthesise pairs of sequences of length 128.  You can specify the sequence length through the seq_length variable (line 8), the size of the dataset you want through the size variable (line 9), as well as the maximum number of mutations (number of nucleotides different between the sequences in each pair through the max_mutations variable (line 10)

2. siameseBERT.ipynb was the initial attempt to use a Siamese BERT network to train a diverging hashing function which was then adapted to obtain primers of the right melting temperature.  This attempt was not successful and adapted to make siameseNN.ipynb

3. siameseNN.ipynb is a Siamese neural network that was successful in creating a hashing function that takes an input of maximum length 128 and outputs a primer sequence of the specified length and melting temperature.  Importantly, this is a diverging hashing function where similar inputs lead to different outputs.
- to hash a sequence, first load a trained model using regressor = load_regressor(config).  In this case, to use the model provided, config = “tuned”.
- then call regressor.hash(filename, number_of_loops, temp, primer_length) to hash a file with multiple sequences to be hashed (one.csv is provided as an example of how to format the data)
- or call regressor.hash_one_seq(seq, number_of_loops, temp, primer_length) to hash one sequence only

4. encode.ipynb was helper code generously provided by Dr Thomas Heinis.  This was included to show how data is first encoded using encode.ipynb, then fed through the hashing function in siameseNN.ipynb to generate primers.

Additionally, 2500.csv was included as an example dataset that was generated using make_training_data.ipynb.  This can be used to train the model in siameseBERT.ipynb.  part100.tbl was also included as an example dataset that can be encoded using encode.ipynb, then fed into the hashing function in siameseNN.ipynb.  tunedmodel.pickle is a model trained using siameseNN.ipynb when the hyperparameters were set to the optimal ones found using raytune.
