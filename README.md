Generate a bingo board prior to MIT Mystery Hunt! Refresh the page to get a new board.

Inspired by the [several](https://www.darthsanddroids.net/bingo/Episode7/) [Star Wars](https://www.darthsanddroids.net/bingo/Episode8/) [bingos](http://www.darthsanddroids.net/bingo/Episode9/) made by Darths & Droids for the sequel trilogy.

<table border="1" cellpadding="0" cellspacing="0">
    <tr>
        <td width="125" height="125" id="00"></td>
        <td width="125" height="125" id="01"></td>
        <td width="125" height="125" id="02"></td>
        <td width="125" height="125" id="03"></td>
        <td width="125" height="125" id="04"></td>
    </tr>
    <tr>
        <td width="125" height="125" id="10"></td>
        <td width="125" height="125" id="11"></td>
        <td width="125" height="125" id="12"></td>
        <td width="125" height="125" id="13"></td>
        <td width="125" height="125" id="14"></td>
    </tr>
    <tr>
        <td width="125" height="125" id="20"></td>
        <td width="125" height="125" id="21"></td>
        <td width="125" height="125" id="22">Waiting for Javascript to populate entries. Please wait...</td>
        <td width="125" height="125" id="23"></td>
        <td width="125" height="125" id="24"></td>
    </tr>
    <tr>
        <td width="125" height="125" id="30"></td>
        <td width="125" height="125" id="31"></td>
        <td width="125" height="125" id="32"></td>
        <td width="125" height="125" id="33"></td>
        <td width="125" height="125" id="34"></td>
    </tr>
    <tr>
        <td width="125" height="125" id="40"></td>
        <td width="125" height="125" id="41"></td>
        <td width="125" height="125" id="42"></td>
        <td width="125" height="125" id="43"></td>
        <td width="125" height="125" id="44"></td>
    </tr>
</table>

<div style="text-align:center">
    <input id="seed" type="text">
    <button id="generate">Generate</button>
</div>

<script>
var PHRASE_LIST = [
    "Puzzle about Magic: the Gathering.",
    "Puzzle requires playing out a board game.",
    "Answer to the puzzle appears in the title.",
    "Puzzle uses ternary in extraction.",
    "Something given at the start of Hunt is a puzzle.",
    "Puzzle that requires physically running around.",
    "A puzzle is part of at least two metapuzzles.",
    "Puzzle about Taylor Swift.",
    "Puzzle is stuck for 30+ minutes, then someone checks the work and solves it.",
    "Puzzle is stuck for 4+ hours, then gets backsolved.",
    "Puzzle references previous Mystery Hunts.",
    "Puzzle uses a video game released in the past 2 years.",
    "Hunt is won on Monday (Eastern time zone).",
    "Puzzle about Harry Potter.",
    "Puzzle is a Konundrum.",
    "Puzzle uses a TV show that stopped airing at least 30 years ago.",
    "Puzzle that uses blood types.",
    "External tool used for hunt goes down.",
    "First puzzle is solved in the first 10 minutes.",
    "Puzzle uses element symbols.",
    "Puzzle uses grad-level math or higher.",
    "There's a copy-to-clipboard button.",
    "A logic puzzle with more than one solution.",
    "Puzzle data is embedded in something publicly available months ago.",
    "Puzzle about bridge or poker.",
    "Puzzle that references Star Trek.",
    "SCAVENGER HUNT!!!",
    "Metapuzzle solved with <= half the answers.",
    "Hunt is won before Sunday (Eastern time zone).",
    "The winning team has < 60 members.",
    "The winning team has 60+ members.",
    "More than 10 incorrect guesses on a single puzzle.",
    "Puzzle about a webcomic.",
    "A puzzle is part of at least two metapuzzles.",
    "Multiple teams are on the final runaround simultaneously.",
    "Puzzle that uses solfege.",
    "Puzzle involves playing a video game.",
    "Puzzle where anagramming is part of the intended solution.",
    "Puzzle uses a show that started airing in the past 2 years.",
    "Puzzle about Lord of the Rings.",
    "Puzzle requires identifying over 25 audio clips.",
    "Puzzle that references Pokemon.",
    "A puzzle has multiple answers.",
    "Puzzle referencing a Pixar movie.",
    "Puzzle whose crucial step is realizing it matches an MIT landmark.",
    "First meta is solved in the first 2 hours.",
    "A cryptics puzzle where the wordplay half must be modified first.",
    "Puzzle where teams must create a music video.",
    "Puzzle is easier if someone's not in the Greater Boston area.",
    "Puzzle which has the phrase HERRING or RED HERRING",
    "Puzzle that references My Little Pony.",
    "The hunt has 160+ puzzles.",
    "The hunt has < 160 puzzles.",
    "Puzzle is stuck because final step is to solve a cryptic and no one can.",
    "Non-meta puzzle answer is over 20 letters long.",
    "No errata is issued during Hunt.",
    "Tech issues at Hunt start :(",
    "A crossword that's 5x5 or smaller.",
    "Puzzle features a 5x5 grid.",
    "Puzzle includes Luigi (any Luigi).",
    "The word \"death\" is on a puzzle page.",
    "The word \"mayhem\" is on a puzzle page.",
    "A puzzle whose title is only emoji.",
    "For every letter of the alphabet, there's a puzzle starting with that letter.",
    "Puzzle references or uses a large language model.",
    "Diagramless crossword.",
    "Titles of feeder puzzles are relevant to meta.",
    "A N I M E",
    "Puzzle references VTubers.",
    "A puzzle about baseball.",
    "A puzzle about football (either one).",
    "Logic puzzle in more than 2 dimensions.",
    "Puzzle involves song lyrics.",
    "Puzzle uses a non-standard list of 26 things.",
    "Puzzle about 6 7"
];

// From https://github.com/bryc/code/blob/master/jshash/experimental/cyrb53.js
// Generate 53-bit hash
// Should generate enough randomness / be impossible to rig even with source code.
const cyrb53 = (str, seed = 0) => {
  let h1 = 0xdeadbeef ^ seed,
    h2 = 0x41c6ce57 ^ seed;
  for (let i = 0, ch; i < str.length; i++) {
    ch = str.charCodeAt(i);
    h1 = Math.imul(h1 ^ ch, 2654435761);
    h2 = Math.imul(h2 ^ ch, 1597334677);
  }

  h1 = Math.imul(h1 ^ (h1 >>> 16), 2246822507) ^ Math.imul(h2 ^ (h2 >>> 13), 3266489909);
  h2 = Math.imul(h2 ^ (h2 >>> 16), 2246822507) ^ Math.imul(h1 ^ (h1 >>> 13), 3266489909);

  return 4294967296 * (2097151 & h2) + (h1 >>> 0);
};

// From https://github.com/bryc/code/blob/master/jshash/PRNGs.md#mulberry32
// Seedable PRNG.
function mulberry32(a) {
    return function() {
      a |= 0; a = a + 0x6D2B79F5 | 0;
      var t = Math.imul(a ^ a >>> 15, 1 | a);
      t = t + Math.imul(t ^ t >>> 7, 61 | t) ^ t;
      return ((t ^ t >>> 14) >>> 0) / 4294967296;
    }
}


function cleanSeed(seed) {
    var cleaned = seed.replace(/[^0-9a-zA-Z]/g, '');
    cleaned = cleaned.toUpperCase();
    return cleaned;
}

function shuffle(array, prng) {
    var currentIndex = array.length
      , temporaryValue
      , randomIndex
      ;

    // While there remain elements to shuffle...
    while (0 !== currentIndex) {

      // Pick a remaining element...
      randomIndex = Math.floor(prng() * currentIndex);
      currentIndex -= 1;

      // And swap it with the current element.
      temporaryValue = array[currentIndex];
      array[currentIndex] = array[randomIndex];
      array[randomIndex] = temporaryValue;
    }

    return array;
}

function randomSeed() {
    const chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789';
    let lst = [];
    for (var i = 0; i < 9; i++) {
        lst[i] = chars.charAt(Math.floor(Math.random() * chars.length));
    }
    return lst.join('');
}

// Write a random seed value.
// This is needed to make it work properly on refresh - the browser seems to cache
// the input value which makes it pass the check in generate()
var seedElem = document.getElementById('seed');
seedElem.value = randomSeed();

function generate() {
    if (!seedElem.value) {
        // Generate for them
        console.log(seedElem.value);
        seedElem.value = randomSeed();
    }
    var cleaned = cleanSeed(seedElem.value);
    var prng = mulberry32(cyrb53(cleanSeed(seedElem.value)));

    var phraseList;
    if (cleaned === 'NOISREVHM') {
        phraseList = [
            "Puzzle where teams must create a music video.",
            "Puzzle about Magic: the Gathering.",
            "Puzzle about a webcomic.",
            "Puzzle about Taylor Swift.",
            "Puzzle is a Konundrum.",
            "Puzzle whose crucial step is realizing it matches an MIT landmark.",
            "Puzzle uses grad-level math or higher.",
            "Something given at the start of Hunt is a puzzle.",
            "No errata is issued during Hunt.",
            "Puzzle where anagramming is part of the intended solution.",
            "Puzzle references previous Mystery Hunts.",
            "Puzzle requires playing out a board game.",
            "Puzzle uses a video game released in the past 2 years.",
            "Puzzle uses element symbols.",
            "Multiple teams are on the final runaround simultaneously.",
            "A puzzle is part of at least two metapuzzles.",
            "Hunt is won on Monday (Eastern time zone).",
            "First meta is solved in the first 2 hours.",
            "Puzzle involves playing a video game.",
            "Puzzle which has the phrase HERRING or RED HERRING",
            "First puzzle is solved in the first 10 minutes.",
            "Puzzle uses a TV show that stopped airing before 1990.",
            "Puzzle uses an anime that started airing in the past 2 years.",
            "Hunt is won before Sunday (Eastern time zone).",
        ];
    } else {
        // Shuffle then take first 24 entries.
        phraseList = [...PHRASE_LIST];
        phraseList = shuffle(phraseList, prng);
    }

    var count = 0;
    for (i = 0; i < 5; i++) {
        for (j = 0; j < 5; j++) {
            // Assign entries
            var id = i.toString() + j.toString();
            var element = document.getElementById(id);
            if (i === 2 && j === 2) {
                element.innerHTML = "FREE SQUARE: \"This is not a puzzle.\"";
                element.style.fontWeight = "bold";
            } else {
                element.innerHTML = phraseList[count++];
            }
            // Misc styling
            element.style.textAlign = "center";
            element.style.verticalAlign = "middle";
        }
    }
}

// connect to button and generate intial page
document.getElementById('generate').onclick = function() { generate(); }
generate();
</script>
