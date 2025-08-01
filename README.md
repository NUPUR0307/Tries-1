# Tries-1
TC : O(L)
SC : O(N*L)

//Approach:-
1. We build the Trie where isInsert is used to insert the word and mark the end as true to denote that we have a word

## Problem1 
Implement Trie (Prefix Tree)(https://leetcode.com/problems/implement-trie-prefix-tree/)

class Trie {
    class Node {
        Node[] arr;
        boolean flag;

        public Node() {
            this.arr = new Node[26];
            flag = false;
        }

        public boolean isEnd() {
            return flag;
        }
    }

    Node root;

    public Trie() {
        this.root = new Node();
    }

    public void insert(String word) {
        Node node = root;
        for (int i = 0; i < word.length(); i++) {
            char c = word.charAt(i);
            if (node.arr[c - 'a'] == null) {
                node.arr[c - 'a'] = new Node();
            }
            node = node.arr[c - 'a'];
        }
        node.flag = true; 
    }

    public boolean search(String word) {
        Node node = root;
        for (int i = 0; i < word.length(); i++) {
            char c = word.charAt(i);
            if (node.arr[c - 'a'] == null) return false;
            node = node.arr[c - 'a'];
        }
        return node.isEnd();
    }

    public boolean startsWith(String prefix) {
        Node node = root;
        for (int i = 0; i < prefix.length(); i++) {
            char c = prefix.charAt(i);
            if (node.arr[c - 'a'] == null) return false;
            node = node.arr[c - 'a'];
        }
        return true;
    }
}



## Problem2
TC : O(N)
SC : O(N)

//Approach:-
1. We build the Trie where insert is used to insert the word and mark the end as true to denote that we have a word
2. We will use dfs+backtrack to find the longest word 
Longest Word in Dictionary(https://leetcode.com/problems/longest-word-in-dictionary/)

class Solution {
    TrieNode root;
    String result;

    class TrieNode {
        TrieNode[] children;
        boolean isEnd;

        public TrieNode(){
            this.children = new TrieNode[26];
        }
    }

    private void insert(String word){
        TrieNode curr = root;
        for(char c: word.toCharArray()){
            if(curr.children[c - 'a'] == null){
                curr.children[c - 'a'] = new TrieNode();
            }
            curr = curr.children[c - 'a'];
        }
        curr.isEnd = true;
    }

    public String longestWord(String[] words) {
        this.root = new TrieNode();
        this.result = "";

        for(String word : words){
            insert(word);
        }

        dfs(root, new StringBuilder());

        return result;
    }

    private void dfs(TrieNode curr, StringBuilder path){

        if(result.length() < path.length()){
            result = path.toString();
        }

        for(int i=0; i<26; i++){
            if(curr.children[i] != null && curr.children[i].isEnd){

                int le = path.length();

                //action
                path.append((char)(i+'a'));

                //recurse
                dfs(curr.children[i], path);

                //backtrack
                path.setLength(le);

            }
        }
    }
}


## Problem3
TC : O(N)
SC : O(N)

//Approach:-
1.	Build a Trie with all root words from the dictionary.
2.	For each word in the sentence, traverse the Trie to find the shortest prefix match.
3.	Construct the result sentence by replacing words with their root prefix if found.
Replace Words (https://leetcode.com/problems/replace-words/)


class Solution {

    TrieNode root;

    class TrieNode {
        TrieNode[] children;
        boolean isEnd;

        public TrieNode() {
            this.children = new TrieNode[26];
        }
    }

    private void insert(String word) {
        TrieNode curr = root;
        for(char c : word.toCharArray()) {
            if(curr.children[c - 'a'] == null) {
                curr.children[c - 'a'] = new TrieNode();
            }
            curr = curr.children[c - 'a'];
        }
        curr.isEnd = true;
    }

    public String replaceWords(List<String> dictionary, String sentence) {
        this.root = new TrieNode();
        for(String word : dictionary) {
            insert(word);
        }
        StringBuilder result = new StringBuilder();

        String[] splitArr = sentence.split(" ");

        for(int i = 0; i < splitArr.length; i++) {
            String word = splitArr[i];
            if(i > 0) result.append(" ");
            result.append(getShortestVersion(word));
        }

        return result.toString();
    }

    private String getShortestVersion(String word) {
        TrieNode curr = root;
        StringBuilder sb = new StringBuilder();

        for(char c : word.toCharArray()) { 
            if(curr.children[c - 'a'] == null || curr.isEnd) {
                break;
            }
            curr = curr.children[c - 'a'];
            sb.append(c);
        }

        if(curr.isEnd) {
            return sb.toString();
        }

        return word;
    }
}
