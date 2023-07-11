#include <iostream>
#include <string>
#include <utility>
  
  
  
  
  
  
class MyString { 
  
  
  
public:    
  
  
  
  
    MyString() : data(nullptr), size(0) {}
    MyString(const char* s) : size(strlen(s)), data(new char[size + 1]) { memcpy(data, s, size + 1); }
    ~MyString() { delete[] data; }

    MyString(const MyString& other) : size(other.size), data(new char[size + 1]) { memcpy(data, other.data, size + 1); }
    MyString(MyString&& other) noexcept : size(other.size), data(other.data) { other.data = nullptr; }

    MyString& operator=(const MyString& other) {
        if (this != &other) {
            delete[] data;
            size = other.size;
            data = new char[size + 1];
            memcpy(data, other.data, size + 1);
        }
        return *this;
    }

    MyString& operator=(MyString&& other) noexcept {
        if (this != &other) {
            delete[] data;
            size = other.size;
            data = other.data;
            other.data = nullptr;
        }
        return *this;
    }

    const char* c_str() const { return data; }

private:
  
  
  
    char* data;
    size_t size;
};

int main() {
    MyString s1("Hello");
    MyString s2(std::move(s1));
    std::cout << s2.c_str() << std::endl;

    MyString s3("World");
    s1 = std::move(s3);
    std::cout << s1.c_str() << std::endl;

    return 0;
}
