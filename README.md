# concordance

import java.util.*;
import java.io.*;

/**
 * The Concordance class mimics a Concordance in that it provides all of the words and
 * their locations within a file as opposed to a book.
 *
 * @author Asma Awad
 * @version 1.0
 */
public class Concordance {
    private Map<String, List<Token>> concordance;

    Concordance(String filename) throws IOException {
        concordance = add(filename); //add initializes the concordance as it loads each of the Tokens in the given file
    }

    /**
     * add creates a Concordance out of a given file by mapping each of the individual
     * words in the file to its location i.e through "Tokenizing" the file into individual
     * Tokens.
     * @param filename the file to be parsed into individual
     * @return a map mapping each of the words in the file to its locations
     * @throws IOException in the case of issues with file handling
     */
    public Map<String, List<Token>> add(String filename) throws IOException {
        Tokenizer t = new Tokenizer(filename);
        Token token;
        concordance = new TreeMap<>(); //using a TreeMap to have the concordance in alphabetical order

        //while there are tokens to be read from the tokenized file
        while (t.hasNext()) {

            token = t.next();
            //if the concordance does not already have the word
            if (!(concordance.containsKey(token.getWord()))) {
                //add it in and map it to an ArrayList containing the location of its first occurrence
                concordance.put(token.getWord(), new ArrayList<>());
                concordance.get(token.getWord()).add(token);
            }
            //otherwise, simply add the word's next location into the ArrayList
            else
                concordance.get(token.getWord()).add(token);

        }
        return concordance;
    }

    /**
     * toString provides a formatted representation of a Concordance
     * @return a String representing the Concordance
     */
    public String toString() {
        String concordanceStr = "";

        //prints the word followed by its locations
        for (Map.Entry<String, List<Token>> entry : concordance.entrySet()) {
            concordanceStr += entry.getKey() + "\n";
            for (Token t : entry.getValue()) {
                concordanceStr += "\t" + t.toString() + "\n";
            }
        }

        return concordanceStr;
    }

    /**
     * count returns how many times a given word occurs in a file
     * @param s the word for which the number of occurrences is being determined
     * @return the number of occurrences for the given word
     */
    public int count(String s) {
        return concordance.get(s).size();
    }
}
