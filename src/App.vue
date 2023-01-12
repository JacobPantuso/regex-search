<script>
export default {
      methods: {
        regexSearch: function(){
          this.validate();
          // convert string to regex
          var s = document.getElementById("regex").value;
          const m = s.match(/^([/~@;%#'])(.*?)\1([gimsuy]*)$/);
          var regex = m ? new RegExp(m[2],m[3]) : new RegExp(s, "g");
          var text = document.getElementById("text").value;
          var matches = text.match(regex);
          if (matches == null) {
            document.getElementById("result").innerHTML = "No Matches Found";
            document.getElementById("result-box").style.display = "flex";
            document.querySelector(".match").classList.add("disabled");
            return;
          } else {
            document.getElementById("btn-match").innerHTML = "Matches Found: " + matches.length;
          }
          // highlight matches in green
          var highlighted = text.replace(regex, function(match) {
            return "<mark class='highlight'>" + match + "</mark>";
          });
          var wordList = text.split(/\s+/);
          if (document.getElementById("check").checked == false) {
           for (var i = 0; i < wordList.length; i++) {
            if (!regex.test(wordList[i])) {
                highlighted = highlighted.replace(wordList[i], "");
              }
           }
          }
          highlighted = highlighted.replace(/\n/g, "<br>");
          document.getElementById("result").innerHTML = highlighted;
          document.getElementById("result-box").style.display = "flex";
          document.querySelector(".match").classList.add("disabled");
        },
        reset: function(id) {
          if (id == "regex") {
            this.$refs.regex.reset();
          } else if (id == "text") {
            this.$refs.text.reset();
          }
        },
        validate: function() {
          var s = document.getElementById("regex").value;
          try {
            const m = s.match(/^([/~@;%#'])(.*?)\1([gimsuy]*)$/);
            return m ? !!new RegExp(m[2],m[3])
                : false;
          } catch (e) {
            document.getElementById("regex").style.border = "3px solid red";
            document.querySelector(".match").classList.add("disabled");
          }
        },
        update: function() {
          if (document.getElementById("regex").value == "" || document.getElementById("text").value == "" && (document.getElementById("check").checked == false || document.getElementById("check").checked == true)) {
            document.querySelector(".match").classList.add("disabled");
          } else {
            document.querySelector(".match").classList.remove("disabled");
          }
        },
        regExpEscape: function(string) {
          return string.replace(/[.*+?^${}()|[\]\\]/g, '\\$&');
        },
        report: function() {
          document.getElementById("incon").innerHTML = "<i class='icon fa-solid fa-paper-plane'></i><br>Woosh! Feedback Sent.";
        }
    }
}
</script>

<template>
  <div class="main">
    <h2 class="title">Regex Search</h2>
    <h4 class="desc">Enter your text to search and regex in the given fields below</h4>
    <p class="bug">See a Problem? <a href="mailto:jacob.pantuso@torontomu.ca">Report a Bug</a></p>
    <div class="text-boxes">
      <div class="regex-box">
        <div class="box-name">
          <i class="fa-solid fa-magnifying-glass"></i>
          <h4>Regex</h4>
        </div>
        <form ref="regex"><textarea @keyup="update()" id="regex" name="text" rows="1" cols="60"></textarea></form>
        <button class="btn-reset" @click="reset('regex');">Reset</button>
      </div>
      <div class="search-box">
        <div class="box-name">
          <i class="fa-solid fa-file-lines"></i>
          <h4>Text to Match</h4>
        </div>
        <form ref="text">
          <textarea @keyup="update();" id="text" name="text" rows="1" cols="60"></textarea>
          <div class="radio">
            <input @change="update()" id="check" type="checkbox" value="option">
            <label for="option">Include words that are not matched?</label>
          </div>
        </form>
        <button class="btn-reset" @click="reset('text')">Reset</button>
      </div>
    </div>
    <div id="result-box" class="result-box">
        <div class="box-name">
          <i class="fa-solid fa-square-poll-vertical"></i>
          <h4>Result</h4>
        </div>
        <p id="result" class="result-text"></p>
        <p id="incon" class="bug">Not happy with these results? <a href="#" @click="report();">Report an inconsistency</a></p>
        <button class="btn-reset" onclick="window.location.reload()" >Restart</button>
      </div>
    <button id="btn-match" class="disabled match" @click="regexSearch();">Match Text</button>
  </div>
  <footer>
    &copy 2023 - Jacob Pantuso
  </footer>
</template>