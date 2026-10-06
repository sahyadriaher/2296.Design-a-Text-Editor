#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX 800005

typedef struct {
    char left[MAX];
    char right[MAX];

    int leftSize;
    int rightSize;
} TextEditor;

TextEditor* textEditorCreate() {
    TextEditor* obj = malloc(sizeof(TextEditor));

    obj->leftSize = 0;
    obj->rightSize = 0;

    return obj;
}

void textEditorAddText(TextEditor* obj, char* text) {
    int len = strlen(text);

    for (int i = 0; i < len; i++) {
        obj->left[obj->leftSize] = text[i];
        obj->leftSize++;
    }
}

int textEditorDeleteText(TextEditor* obj, int k) {
    int deleted = 0;

    while (k > 0 && obj->leftSize > 0) {
        obj->leftSize--;
        k--;
        deleted++;
    }

    return deleted;
}

char* getLastTen(TextEditor* obj) {
    int count = obj->leftSize;

    if (count > 10)
        count = 10;

    char* result = malloc(count + 1);

    int start = obj->leftSize - count;

    for (int i = 0; i < count; i++) {
        result[i] = obj->left[start + i];
    }

    result[count] = '\0';

    return result;
}

char* textEditorCursorLeft(TextEditor* obj, int k) {
    while (k > 0 && obj->leftSize > 0) {

        obj->right[obj->rightSize] =
            obj->left[obj->leftSize - 1];

        obj->rightSize++;
        obj->leftSize--;

        k--;
    }

    return getLastTen(obj);
}

char* textEditorCursorRight(TextEditor* obj, int k) {
    while (k > 0 && obj->rightSize > 0) {

        obj->left[obj->leftSize] =
            obj->right[obj->rightSize - 1];

        obj->leftSize++;
        obj->rightSize--;

        k--;
    }

    return getLastTen(obj);
}

void textEditorFree(TextEditor* obj) {
    free(obj);
}
