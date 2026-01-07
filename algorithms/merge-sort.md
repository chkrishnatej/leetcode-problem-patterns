# Merge Sort



## Algorithm

```java
import java.util.*;

class MergeSort {
    public static void main(String[] args) {
        MergeSort ms = new MergeSort();
        
        int[] arr = {38, 27, 43, 3, 9, 82, 10};
        
        System.out.println("Before mergesort");
        for(int ele: arr) System.out.println(ele);
        
        ms.mergeSort(arr);
        
        System.out.println("After mergesort");
        for(int ele: arr) System.out.println(ele);
    }
    
    private void mergeSort(int[] nums) {
        if(nums == null || nums.length < 2) {
            throw new IllegalArgumentException("Invalid input is provided");
        }
        sort(nums, 0, nums.length-1);
    }
    
    private void sort(int[] nums, int left, int right) {
        if(left < right) {
            // Find the middle point
            int mid = left + (right - left) / 2;
            
            // Divide: Sort first and second halves
            sort(nums, left, mid);
            sort(nums, mid+1, right);
            
            // Conquer: Merge the sorted halves
            merge(nums, left, mid, right);
        }
    }
    
    private void merge(int[] nums, int left, int mid, int right) {
        // Sizes of two subarrays to be merged
        int n1 = mid - left + 1;
        int n2 = right - mid;
        
        // Temporary array
        int[] L = new int[n1];
        int[] R = new int[n2];
        
        // Copying the left subarray
        for(int i=0; i<n1; i++) {
            L[i] = nums[left+i];
        }
        // Copying the right subarray
        for (int j = 0; j < n2; j++) {
            R[j] = nums[mid + 1 + j];
        }
        
        // Merging the temp arrays
        int i = 0, j = 0;
        int k = left; // Initial index of the merged subarray 
        
        while(i < n1 && j < n2) {
            // At every iteration, a number is found and stored in the nums
            if(L[i] <= R[j]) { // Relative order and stability is maintained
                nums[k] = L[i++];
            } else {
                nums[k] = R[j++];
            }
            k++; // Moving forward the index
        }
        
        // Copy the remaining subarrys
        while(i < n1) {
            nums[k++] = L[i++];
        }
        
        while(j < n2) {
            nums[k++] = R[j++];
        }
    }
}
```
