# Hashcat

### Tool

{% embed url="https://github.com/hashcat/hashcat" %}

### Hashes list

{% embed url="https://hashcat.net/wiki/doku.php?id=example\_hashes" %}

### Parameters

* -m: Mode \(hash type\)
* -a: Attack type
  * 0 = Straight \(dictionary\)
  * 1 = Combination
  * 2 = Toggle-Case
  * 3 = Brute-force
  * 4 = Permutation
  * 5 = Table-Lookup
  * 8 = Prince
* -o: Output \(if you want it to be saved in a txt\)

### Attack examples

```bash
#Dictionary attack
hashcat -m 1800 -a 0 shadow.txt /usr/share/wordlists/rockyou.txt

#Brute-force attack
hashcat -m 1800 -a 3 shadow.txt
```

### Complete guide about hashcat use \(in spanish\)

{% embed url="https://jesux.es/cracking/passwords-cracking/" %}



