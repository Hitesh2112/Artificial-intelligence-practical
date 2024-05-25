## Resolution in AI using Predicate Logic
```python
def resolver(s, st):
    sum_val = 0
    for i in range(len(s)):
        if 'A' <= s[i] <= 'Z':
            if i != 0 and s[i - 1] == '~':
                st += s[i - 1]
                sum_val += -ord(s[i])
            else:
                sum_val += ord(s[i])
            st += s[i] + ' '
    return sum_val

def cnf_converter(s):
    ans = ''
    flag = False
    for i in range(len(s)):
        if s[i] == '-':
            ans += ' or '
            flag = True
        elif s[i] == '>':
            continue
        else:
            ans += s[i]
    if flag:
        if s[0] != '~':
            temp = '~' + ans
            ans = temp
        else:
            ans = ans[1:]
    return ans

def negate(s):
    return '~' + s

def resolution(arr, mp, goal, resol):
    a = 0
    for i in range(len(arr)):
        t = cnf_converter(mp[arr[i]])
        a += resolver(t, resol)
    g = '~' + mp[goal]  # negate the goal
    a += resolver(g, resol)
    return a == 0

if __name__ == "__main__":
    arr = [
        "If maid stole the jewellery then butler wasn't guilty.",
        "The maid stole the jewellery or she eats the lunch.",
        "If the maid eat the lunch then butler got the cream.",
    ]

    goal = "If butler was guilty then he got the cream."

    mp = {
        arr[0]: "stole (maid) -> ~ guilty (butler)",
        arr[1]: "stole (maid) or eat (maid)",
        arr[2]: "eat (maid) -> cream (butler)",
        goal: "~ (guilty (butler) -> cream (butler))"
    }

    resol = ""
    print("Sentences :-\t\t\t\t\tPredicate Logic")
    for i in range(len(arr)):
        print(f"{arr[i]}  ->  {mp[arr[i]]}")
    
    ans = resolution(arr, mp, goal, resol)
    
    print("Goal:", goal)
    print("CNF Clauses are:")
    for i in range(len(arr)):
        t = cnf_converter(mp[arr[i]])
        print(t)
    print("Resolution: ", resol)
    if ans:
        print("Resolved successfully")
    else:
        print("Doesn't resolved")
```
### OUTPUT
![image](https://github.com/Hitesh2112/Artificial-intelligence-practical/assets/97521900/e2d3b875-529a-4841-ab33-0fe19f9f0483)
