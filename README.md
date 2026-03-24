**Introduction**

"Siralim Ultimate" is a deep, monster-catching RPG that combines elements from roguelikes and traditional role-playing games. Developed by Thylacine Studios, it is the latest installment in the Siralim series, known for its expansive creature collection and complex gameplay mechanics. In "Siralim Ultimate," players assume the role of a mage who commands creatures in strategic battles. The game features an almost endless variety of creatures, each with unique traits and abilities, allowing for a wide range of strategic combinations and team setups.

The core gameplay revolves around exploring randomly generated dungeons, battling creatures, and using resources to upgrade your kingdom. This kingdom serves as your base of operations where you can craft new spells, gear, and potions, breed creatures to create more powerful allies, and manage your realm's political and economic aspects. With no level caps and a virtually infinite endgame, "Siralim Ultimate" offers a highly customizable experience that encourages players to experiment and optimize their teams in pursuit of the most powerful combinations.

**Problem**

An Information Retrieval (IR) system can be extremely beneficial in "Siralim Ultimate," where players have access to a vast number of monsters and traits, and need to make strategic decisions about fusing creatures to optimize their teams. Players would be able to input search queries using keywords or filters. For example, they might search for traits like "fire resistance" or specific abilities that enhance attack power. The IR system would process these queries to find monsters that match the criteria. Based on a player’s current roster and their historical preferences (like frequently chosen traits or monsters), the system could suggest potential monsters for fusion that might enhance the player's team performance. When considering fusion, compatibility, and synergies between traits are crucial. The IR system could provide a detailed analysis of how traits from two different monsters might combine or interact, helping players make informed decisions about potential fusions.

Implementing such an IR system in "Siralim Ultimate" would significantly enhance the player experience by reducing the complexity and time needed to research and plan creature fusions, allowing players to focus more on strategic gameplay and less on manual data gathering and analysis.

**Similar program**

During our research, we were able to find similar programs from different games. 

For example: 

&lt;[https://wiki.wargaming.net/en/World_of_Warships](https://wiki.wargaming.net/en/World_of_Warships)>

This website shows the information on all ships within the game “World of Warships”. Since this is a large online game, the information on the web is constantly updating. We were inspired to find great ways to filter large text data. 

&lt;[https://monsterhunterworld.wiki.fextralife.com/Monster+Hunter+World+Wiki](https://monsterhunterworld.wiki.fextralife.com/Monster+Hunter+World+Wiki)>

Graphic display of information on one of the most popular games in the world.

&lt;[https://rpgbot.net/](https://rpgbot.net/)>

&lt;[https://www.analyticsvidhya.com/blog/2022/01/building-language-models-in-nlp/](https://www.analyticsvidhya.com/blog/2022/01/building-language-models-in-nlp/)>

Language model explaining filtering text and retrieval.

&lt;[https://people.ischool.berkeley.edu/~buckland/papers/analysis/node2.html](https://people.ischool.berkeley.edu/~buckland/papers/analysis/node2.html)>

&lt;[https://www.simplilearn.com/tutorials/machine-learning-tutorial/information-retrieval](https://www.simplilearn.com/tutorials/machine-learning-tutorial/information-retrieval)>

The above examples also showed us how we could approach our problem. Either example programs or NLP models to learn from.

**Data Insights**

Our dataset was provided by &lt;siralim-ultimate.rovermicrover.com> which provided a Google sheet that we downloaded as a CSV file. This dataset provides a comprehensive overview of the creatures in "Siralim," including the following attributes: 



1. Name: Name of the creature.
2. Species: Species classification of the creature.
3. Class: Class type, mostly related to the combat role or nature.
4. Hlt (Health): Health stat, indicating the vitality of the creature.
5. Atk (Attack): Attack stat, indicating the damage potential of the creature.
6. Int (Intelligence): Intelligence stat, likely related to magical abilities or other skills.
7. Def (Defense): Defense stat, showing the creature’s resilience.
8. Spd (Speed): Speed stat, indicating the swiftness of the creature.
9. Total: Total of all stat points.
10. R. Depth: Indicates the depth level at which the creature can be found or its rarity.
11. Source: How the creature can be obtained in the game.
12. Trait Name: Name of the specific trait of the creature.
13. Item Name: Associated item with the creature.
14. Trait Description: Description of the creature’s trait.

The Data we chose to use is from public sources. Since the game is not updating any DLC or extra content, we do not have to worry about the consistency of the data. For our program, we will be emphasizing on Name and Trait Description.

**Problem-solving**

We chose to use Python on Jupyter-notebook to solve this problem.

The program begins by importing the necessary libraries: CSV for handling CSV files, os for interacting with the operating system (like creating directories), and word_tokenize from “nltk.tokenize” for tokenizing text.

We are using the stopwords resource from the “nltk.corpus” module, the code loads a set of English stopwords. These are common words like "and", "the", "is", etc., that are usually filtered out during text processing to focus on more meaningful words.

We then defined the define remove stop words function, processed each file in the directory, content splitting, and cleaning. For each file, the cleaned versions of the triggers and effects are printed, along with the filename. This output could alternatively be written back to the file or used in other ways, depending on the need.

The program then initializes the Porter Stemmer, a popular algorithm used for stemming words in English (i.e., reducing them to their base or root form). It specifies a directory from which it will load text files. This directory is expected to contain the text files that will be processed.

Then we created a function that takes a text string and a stemmer object as input. It first tokenizes the text into words, removes stopwords (common words like "and", "the", etc., which don't contribute much meaning), and then applies stemming to reduce each word to its root form. The cleaned and stemmed words are joined back into a string and returned.

Then, the program reads all the text files in the specified directory, assuming they are formatted with triggers and effects separated by a comma. Each file's content is split into trigger and effect parts. Both parts are cleaned and stemmed using the clean_and_stem_text function. The results, along with the original text, are stored in a dictionary called cleaned_data, indexed by the filename.

We then also created a user interaction that prompts the user to enter the filenames they want to analyze, separated by commas. The program loads words from a file list_of_words.txt containing words of interest. Each word is cleaned and stemmed similarly to the file content, and these words are stored in a set called stemmed_word_list for quick lookup.

For each filename entered by the user, the script: Checks if the file exists in cleaned_data.

Retrieves the cleaned effect and identifies common stemmed words between this effect and the stemmed_word_list. If common words are found, it searches for other files that have triggers containing these common words, handling both combinations of two words and single words.

The search_matching_triggers function facilitates this by looking for files where the triggers match the common words under specific conditions (including handling an exception word, if specified).

The program will print details of the files that have triggers matching the cleaned effects of the input files based on the common words. It displays the filename, original text, cleaned trigger, and cleaned effect.

**Experiments**

After selecting eight creatures for our tests, we entered their names into the search bar and received mixed results. Initially, some searches yielded no results, prompting us to reevaluate our code. We discovered that the issue stemmed from failing to convert uppercase letters to lowercase; the search function didn't recognize the words due to case sensitivity. Once we reformatted the word list, the search functioned correctly. We further refined our approach by differentiating between significant terms such as "attack" (a statistic), "attacks" (an action), and "resurrection" (a keyword in many effects). Our program now correctly distinguishes between "attacks" and "attack" after lemmatization, and it recognizes effects involving "resurrection" more accurately. We manually checked the list of creatures generated to calculate precision and recall, finding the results promising and indicative of potential for further enhancements.


<table>
  <tr>
   <td>Creature
   </td>
   <td>Relevant
   </td>
   <td>Retrieved
   </td>
   <td>Relivent Retrieved
   </td>
   <td>Recall
   </td>
   <td>Precision
   </td>
   <td>F1
   </td>
  </tr>
  <tr>
   <td>Cloth Abomination
   </td>
   <td>1
   </td>
   <td>1
   </td>
   <td>1
   </td>
   <td>1.00
   </td>
   <td>1.00
   </td>
   <td>1.00
   </td>
  </tr>
  <tr>
   <td>Unicorn Vivifier
   </td>
   <td>12
   </td>
   <td>19
   </td>
   <td>12
   </td>
   <td>1.00
   </td>
   <td>0.63
   </td>
   <td>0.82
   </td>
  </tr>
  <tr>
   <td>War Giant
   </td>
   <td>3
   </td>
   <td>3
   </td>
   <td>1
   </td>
   <td>0.33
   </td>
   <td>0.33
   </td>
   <td>0.33
   </td>
  </tr>
  <tr>
   <td>Ear Animation
   </td>
   <td>17
   </td>
   <td>3
   </td>
   <td>3
   </td>
   <td>0.18
   </td>
   <td>1.00
   </td>
   <td>0.59
   </td>
  </tr>
  <tr>
   <td>Nexus Amaranth
   </td>
   <td>6
   </td>
   <td>6
   </td>
   <td>5
   </td>
   <td>0.83
   </td>
   <td>0.83
   </td>
   <td>0.83
   </td>
  </tr>
  <tr>
   <td>Gold Golem
   </td>
   <td>10
   </td>
   <td>9
   </td>
   <td>6
   </td>
   <td>0.60
   </td>
   <td>0.67
   </td>
   <td>0.63
   </td>
  </tr>
  <tr>
   <td>Augmented Ossein
   </td>
   <td>92
   </td>
   <td>230
   </td>
   <td>92
   </td>
   <td>1.00
   </td>
   <td>0.40
   </td>
   <td>0.70
   </td>
  </tr>
  <tr>
   <td>Average
   </td>
   <td>
   </td>
   <td>
   </td>
   <td>
   </td>
   <td>0.71
   </td>
   <td>0.69
   </td>
   <td>0.70
   </td>
  </tr>
</table>


**Conclusion**

Using NLTK, we developed a functional search engine designed to assist players in locating fusion materials. The program currently faces challenges with general trigger conditions and struggles to include passive skills and abilities that contain multiple commas. Despite these issues, we managed to achieve decent recall, though precision remains an area for improvement. This search engine is particularly beneficial for new players, who may not be familiar with specific keywords necessary for effective searching. Our solution improves upon existing databases by distinguishing between ability triggers and effects, a feature most Siralim databases lack. This enhancement helps narrow down searches more effectively, aiding players in making informed decisions.

**Contribution**

Himchand Gee: Program designer, researched relevant information on what needs to be included to work effectively, programmer, final report writer.

Yuchong Zhu: Programming and revision of details of the program. Writing of final research paper. Research relevant NLP model.

 
