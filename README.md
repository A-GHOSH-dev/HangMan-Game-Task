# HangMan-Game-Task-Strategy

1. Vowel Count Ratio Analysis:
- The strategy involves analyzing the ratio of vowels to the length of a word.
- If the number of vowels in a word is greater than 55% of its length, it's less likely to have more vowels
```
vowels = ['a', 'e', 'i', 'o', 'u']
def vowel_count(clean_word):
    count = 0
    for i in clean_word:
        if i in vowels:
            count = count + 1.0
    return count / len(clean_word)
# Example usage
clean_word = "example"
vowel_ratio = vowel_count(clean_word)
print(f"Vowel Count Ratio: {vowel_ratio}")
```

2. Word Length Analysis and Substring Dictionary:
- Analyze the length of words and create a dictionary of substrings of various lengths.
- If the algorithm runs out of similar words in the given dictionary for the semi-guessed word, it tries to break the word into smaller fragments (suffix/prefix) to fit in the most appropriate letter.

```
max_length = 0
for words in df:
    if len(words) > max_length:
        max_length = len(words)
n_word_dictionary = {i: [] for i in range(3, 30)}
count = 3
while count <= max_length:
    for words in df:
        if len(words) >= count:
            for i in range(len(words) - count + 1):
                n_word_dictionary[count].append(words[i:i + count])
    count = count + 1
```

3. Frequency Count and Guessing:
- Use a statistical approach to guess the next letter based on frequency.
- The algorithm tries to guess the maximum occurring letter in the current plausible words.

```
def func(new_dictionary):
    dictx = collections.Counter()
    for words in new_dictionary:
        temp = collections.Counter(words)
        for i in temp:
            temp[i] = 1
            dictx = dictx + temp
    return dictx

def func2(n_word_dictionary, clean_word):
    new_dictionary = []
    l = len(clean_word)
    for dict_word in n_word_dictionary[l]:
        if re.match(clean_word, dict_word):
            new_dictionary.append(dict_word)
    return new_dictionary
```

4. Overall Strategy:
- The algorithm follows a step-by-step approach, first trying to match similar words, then breaking down the word into substrings if needed, and finally resorting to the frequency distribution of the entire dictionary.
- clean the word so that we strip away the space characters
- replace "_" with "." as "." indicates any character in regular expressions
- find length of passed word
- grab current dictionary of possible words from self object, initialize new possible words dictionary to empty
- iterate through all of the words in the old plausible dictionary
- continue if the word is not of the appropriate length
- if dictionary word is a possible match then add it to the current dictionary
- overwrite old possible words dictionary with updated version
- return most frequently occurring letter in all possible words that hasn't been guessed yet
- if no word matches in training dictionary, default back to ordering of full dictionary
- If all else fails, the algorithm falls back to the frequency distribution of the entire dictionary.

```
    def guess(self, word): 
        clean_word = word[::2].replace("_",".")
        len_word = len(clean_word)
        current_dictionary = self.current_dictionary
        new_dictionary = []
        for dict_word in current_dictionary:
            if len(dict_word) != len_word:
                continue
            if re.match(clean_word,dict_word):
                new_dictionary.append(dict_word)
        self.current_dictionary = new_dictionary
        c = func(new_dictionary)
        sorted_letter_count = c.most_common()                   
        guess_letter = '!'
        for letter,instance_count in sorted_letter_count:
            if letter not in self.guessed_letters:
                if letter in vowels and vowel_count(clean_word)>0.55:
                    self.guessed_letters.append(letter)
                    continue
                guess_letter = letter
                break
        if guess_letter == '!':
            new_dictionary = func2(n_word_dictionary, clean_word)
            c = func(new_dictionary)
            sorted_letter_count = c.most_common()
            for letter,instance_count in sorted_letter_count:
                if letter not in self.guessed_letters:
                    if letter in vowels and vowel_count(clean_word)>0.55:
                        self.guessed_letters.append(letter)
                        continue
                    guess_letter = letter
                    break
        if guess_letter == '!':
            x = int(len(clean_word)/2)
            if x>=3:
                c = collections.Counter()
                for i in range(len(clean_word)-x+1):
                    s = clean_word[i:i+x]
                    new_dictionary = func2(n_word_dictionary, s)
                    temp = func(new_dictionary)
                    c = c+temp
                sorted_letter_count = c.most_common()
                for letter, instance_count in sorted_letter_count:
                    if letter not in self.guessed_letters:
                        guess_letter = letter
                        break  
        if guess_letter == '!':
            x = int(len(clean_word)/3)
            if x>=3:
                c = collections.Counter()
                for i in range(len(clean_word)-x+1):
                    s = clean_word[i:i+x]
                    new_dictionary = func2(n_word_dictionary, s)
                    temp = func(new_dictionary)
                    c = c+temp
                sorted_letter_count = c.most_common()
                for letter, instance_count in sorted_letter_count:
                    if letter not in self.guessed_letters:
                        guess_letter = letter
                        break
        if guess_letter == '!':
            sorted_letter_count = self.full_dictionary_common_letter_sorted
            for letter,instance_count in sorted_letter_count:
                if letter not in self.guessed_letters:
                    if letter in vowels and vowel_count(clean_word)>0.55:
                        self.guessed_letters.append(letter)
                        continue
                    guess_letter = letter
                    break            
        return guess_letter

```

This strategy incorporates a mix of statistical analysis, pattern matching, and fallback mechanisms to improve accuracy in the Hangman game. Adjusting the parameters and fine-tuning the approach based on further observations may enhance the performance.




















































