# Whiteboarding Exercise


merge sort with a proc not modifying the array

a = [1, 2, 3]

class Array
    def merge_sort(&prc) 
        return self if self.length <= 1
        prc ||= Proc.new { |a, b| a <=> b}

        mid = self.length / 2 

        left = self.take(mid).merge_sort(&prc)    
        right = self.drop(mid).merge_sort(&prc)           

        merge(left, right, &prc)
    end

    def merge(left, right, &prc)
        merged = [] 

        until left.empty? || right.empty?
            if prc.call(left.first, right.first) >= 0
                merged << right.shift
            else
                merged << left.shift
            end
        end
        merged + left + right
    end
end


binary search an array for a target and return index if found; assume array is sorted; write recursively

def bsearch(sorted_array, target)                                                       # bsearch( [1,2,3,4], 5), paused on line 50
    return nil if sorted_array.empty?

    probe_index = sorted_array.length / 2                                               # p_i = 2; 0
    probe_value = sorted_array[probe_index]                                             # p_v = 3; 4

    case probe_value <=> target                                                         # 3 <=> 5; 4 <=> 5
    when 0
        probe_index
    when -1
        #probe value is less than target, so search in the right side
        right = sorted_array[probe_index + 1..-1]                                       # right = [4];   right = [4][1..-1] = []
        intermediate_result = bsearch(right, target)                                    # bsearch([4], 5), pauses on line 50; bsearch([], 5)
        intermediate_result.nil? ? nil : (probe_index + 1) + intermediate_result
    when 1
        #probe value is larger than target, so search in the left side
        left = sorted_array[0...probe_index]
        bsearch(left, target)
    end
end


monkey patching my flatten an Array to a specified level, if not 
specified it should flatten it all.
no argument = one D array

```ruby
# Without an argument:
[["a"], "b", ["c", "d", ["e"]]].my_flatten = ["a", "b", "c", "d", "e"]

# When given 1 as an argument:
# (Flattens the first level of nested arrays but no deeper)
[["a"], "b", ["c", "d", ["e"]]].my_flatten(1) = ["a", "b", "c", "d", ["e"]]
```
class Array
    def my_flatten(level = nil)
        return self if level == 0

        if level.nil?
            flattened = []
            self.each do |ele| 
                if ele.is_a?(Array)
                    flattened += ele.my_flatten
                else 
                    flattened << ele
                end
            end
            return flattened
        else
            flattened = []
            self.each do |ele|
                if !ele.is_a?(Array)
                    flattened << ele
                else
                    flattened += ele.my_flatten(level - 1)      
                end
            end
            flattened
        end
    end
end


monkey patch my_each, passes a block to perform on each element in the array

class Array
    def my_each(&prc)                                   # sum = 0
        idx = 0                                         # [1,2,3].my_each { |ele| sum += ele }
        while idx < self.length
            current_ele = self[idx]                    
            prc.call(current_ele)

            idx += 1
        end
        self
    end
end


sum = 0

[1,2].each { |num| sum += num }

sum