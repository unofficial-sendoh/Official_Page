# Print Folder Structure

[Explanation Video Link on Youtube](https://youtu.be/Emr9lseZcRM)

```cpp
#include <vector>
#include <string>
#include <iostream>
#include <map>

struct Trie
{
    std::map<std::string, Trie *> childs;
    std::string val;

    Trie(const std::string &val_in)
    {
        val = val_in;
    }

    Trie *add_child(const std::string &val)
    {
        if (!childs.count(val))
        {
            childs[val] = new Trie(val);
        }

        return childs.at(val);
    }
};

class FileSystem
{
private:
    Trie *root_;

public:
    FileSystem(const std::vector<std::string> &files)
    {
        root_ = new Trie("");

        for (const auto &file : files)
        {
            Trie *cur_node = root_;
            int cur_index = 0;
            std::string cur_folder_name = "";

            for (int cur_index = 0; cur_index < file.size(); cur_index++)
            {
                if (file[cur_index] == '/')
                {

                    if (!cur_folder_name.empty())
                    {
                        cur_node = cur_node->add_child(cur_folder_name);
                    }

                    cur_folder_name = "/";
                }
                else if (cur_index + 1 == file.size())
                {
                    cur_folder_name += file[cur_index];
                    cur_node = cur_node->add_child(cur_folder_name);
                }
                else
                {
                    cur_folder_name += file[cur_index];
                }
            }
        }
    }

    void pre_order_travasel(Trie *node, std::string leading_tab)
    {
        std::cout << leading_tab << node->val << std::endl;
        leading_tab += "  ";

        for (const auto &it : node->childs)
        {
            pre_order_travasel(it.second, leading_tab);
        }
    }

    void print_folder_structure()
    {
        pre_order_travasel(root_, "");
    }
};

int main()
{
    std::vector<std::string> files = {
        "/app/lib/header/foo.h",
        "/app/lib/header/snake.h",
        "/app/lib/cpp/foo.cpp",
        "/app/src/main.cpp"};

    FileSystem file_system{files};
    file_system.print_folder_structure();
}
```
