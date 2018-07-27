# Pancake Sorting
"I want my pancakes to be sorted, otherwise I can't eat them!" A computer scientist yells.

## Background and Idea  
Above is just my personal assumption where this problem comes from. In [wikipedia](https://en.wikipedia.org/wiki/Pancake_sorting), it is said to be first brought up by American geometer Jacob E. Goodman. This is an interesting problem, because unlike array elements, pancakes cannot be swapped. 
What we could do is to insert [spatula](https://en.wikipedia.org/wiki/Spatula) at any point and flip(in computer science, this is called reversal) all pancakes above it. Thus, unlike traditional sorting algorithms which focu on reducing times of comparison, here we pay more attention to times of reversals.

Below is one idea of one implementation:
1. Find the largest pancake among all pancakes
2. Stick a spatula into it and flip
3. Flip the whole stack of pancakes. Now the largest pancake is at the end.
4. Repeat 1-3 steps, now we only consider all pancakes except the last one. Keep doing it until there is no pancakes left.  

Here is a video showing how it is done: [Pancake Sorting](https://www.youtube.com/embed/kk-_DDgoXfk)

## Implementation
``` java
class Solution{
    public void pancakeSort(int[] pancakes){
        sortHelper(pancakes, pancakes.length - 1);
    }

    private void sortHelper(int[] pancakes, int n){
        if(n == 0){
            return;
        }
        int largest = findMax(pancakes, 0, n);
        flip(pancakes, 0, largest);
        flip(pancakes, 0, n);
        sortHelper(pancakes, n - 1);
    }

    // return the index of largest pancake in current subarray
    private int findMax(int[] pancakes, int start, int end){
        int index = start;
        int max = pancakes[start];

        for(int i = start + 1; i <= end; i++){
            if(pancakes[i] > max){
                max = pancakes[i];
                index = i;
            }
        }
        return index
    }

    private void flip(int[] pancakes, int start, int end){
        int len = end - start + 1;
        for(int i = 0; i < len / 2; i++){
            swap(pancakes, start + i, end - i);
        }
    }

    // I said there is no swapping in terms of pancakes, but in current
    // computer architecture, there is no "flipping" operation, so we 
    // need to use swap to simulate it
    private void swap(int[] pancakes, int i, int j){
        int temp = pancakes[i];
        pancakes[i] = pancakes[j];
        pancakes[j] = temp;
    }
}

```


## Reference:
* [Geekforgeeks](https://www.geeksforgeeks.org/pancake-sorting/)
* [Wikipedia](https://en.wikipedia.org/wiki/Pancake_sorting)