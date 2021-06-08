# SEARCH ENGINE

Create an *Inverted Index* to store metadata of documents (application entities) containing words as follows:

    {
        word: [[<doc_id>, [<positions>]]]
    }

Example:

    {
        'hello': [
            [1, [5, 10, 25]],
            [5, [15, 40]]
        ]
    }

Here,  
Document 1 contains the word `hello` at positions (indexes) 5, 10 and 25.
Document 5 contains the word `hello` at positions (indexes) 15 and 40.
 
