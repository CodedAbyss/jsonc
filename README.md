# jsonc
dead simple json parser / serializer in < 300 LOC

Example:

test.json:
```
{ "response": { "id": 123, "arr": [ "a", "b", "c" ] } }
```
main.c:
```c
#include "json.h"
#include <stdio.h>
int main(int argc, char **argv) {
    char field[64], value[64];
    if(!json_read_file("test.json")) return printf("failed to read test.json"), 0;
    json_obj_iter(field, 64) {
        int id;
        if(!strcmp(field, "id")) {
            if(json_int(&id))
                printf("parsed id: %d\n", id);
            else printf("malformed id\n");
        } else if(!strcmp(field, "arr")) {
            json_arr_iter() {
                if(json_str(value, 64))
                    printf("arr entry: \"%s\"\n", value);
            }
        } 

    }
    json_end();
    json_reset();
}
