#include<stdio.h>
#include<stdlib.h>
//yaha humne node create ki hai
struct node{
    int data;
    struct node* next;
};
//ye ek function bnaya hai jo linked list ko print karega
void printll(struct node* head)
{
    
    while (head != NULL)
    {
        printf("Element :%d\tstored at %d\tcontains %d in its next part\n", head->data, head, head->next);
        head = head->next;//is line ka matlab hai ki hum head ko aage aage badha rahe h mtlb pehle head 1 ko point kar rha tha or fir 2 ko or fir ko aise aise tab tak karega jab tak loop chlega
    }
    
}

//struct node* ye function ka (return type hai) jo ki pointer return karega node type ka 
//is function se hum shuruwat me node ko add karenge
struct node* addfirst(struct node* head, int data)
{
    struct node* newnode = (struct node*)malloc(sizeof(struct node));//yaha hum node ko memory dila rahe hai
    
    if (head==NULL)//yaha hum check kar rahe hai ki 1 bhi node exist karti hai ya nhi
    {
        newnode->data = data;//yaha hum node ke data part me data daal rahe hai
        newnode->next = NULL;//yaha hume node ke next part me null daal rahe kyuki abhi 1 bhi node hi hai 
        return newnode;
    }
    else
    {
        newnode->data = data;
        newnode->next = head;//is part me tab control aayega jab already node ho tab hum newnode ke next part m head ka address daal rahe h
        head = newnode;//hum newnode ko head bna rahe h
        return head;
    }
}

struct node* addlast(struct node* head, int data)
{
    struct node* newnode = (struct node*)malloc(sizeof(struct node));
    struct node*  ptr = head;
    newnode->data = data;
    newnode->next = NULL;//new node ke next part m null daaal rahe h kyuki ab ye last node bnegi 
    if (ptr == NULL)//checking ki node h bhi ya nhi
    {
        return newnode;//nhi h toh jo newnode h wahi return kar rhe h
    }
    while (ptr->next != NULL)//agr nodes h toh last tak phauchenge loop ki madad se
    {
        ptr = ptr->next;//hum ptr ko khiska rahe mtlb pehle ptr 1st node ko point kar h fir 2 ko jab tk null na mil jaye fir ptr last node ko point karega ab
    }
    ptr->next = newnode;//abhi ptr last node ko point kar rha h jisme null hai toh ab hum last node ke next part m newnode ka address daal rhe h taaki node ko connect kar sake 
    return head;
}

struct node* addbetween(struct node* head, int data, int index)
{
    int i = 1;
    struct node* newnode = (struct node*)malloc(sizeof(struct node));
    struct node* previousnode = head;//humne naya variable bnaya previous node naam se taaki hum head ko preserve kar sake 
    newnode->data = data;
    while (i < index - 1 && previousnode != NULL)//yaha hum uss index tak phauch rhe h jaha nodeinsert karni h
    {
        previousnode = previousnode->next;//wahi nodes ko khiska rahe h 
        i++;
    }
    newnode->next = previousnode->next;//yahaa jis index pe insert karna h uske next part me wo daal rahe h jo pehle uss node ke pass tha jo uss position pe thi
    previousnode->next = newnode;//yaha hum index se pehle wali node ko newnode jo ki index pe add hine wali h uska address daal rahe h
    return head;
}

struct node* deletefirst(struct node* head)//struct node* head ka mtlb h ki yaha humne head ka address pass kiya h
{
    struct node* ptr = head;//yaha hum ek new variable bna rahe hai taaki head ko preserve kar sake aur us new variable me head ka address assign kar rhe h
    if (ptr == NULL)//yaha hum check kar rhe h ki node h bhi ya nhi
    {
        printf("under flow");//agr node h hi nhi toh print hoga under flow
        return NULL;//yaha hum null return kar rhe h 
    }
    
    head = head->next;//yaha humne 2nd node ko head bna diya kyuki first node delete hone wali h (ye tab hi hoga jab nodes hongi)
    free(ptr);//yaha humne delete kar diya memory se first node ko
    return head;//yaha se humne new head ko return le liya
}

struct node* deletelast(struct node* head)//last wali node delete hogi
{
    if (head == NULL)//agr node hi nhi hai ek bhi
    {
        return NULL;
    }
    if (head->next == NULL)//iska mltb h ki 1 hi node hai agr aisa h toh usse free kar do 
    {
        free(head);//is statement se wo node free ho jayegi
        return NULL;
    }
    
    
    struct node* ptr = head;
    struct node* pn;
    while (ptr->next != NULL)//agr bhot saari node h toh ab hum last node tak chlenge jise hum (ptr me store karenge) or second last node ko (pn me store karenge)
    {
        pn = ptr;//pn second last node ko point kar rahi h
        ptr = ptr->next;//ptr last wali node ko point kargi
    }
    pn->next = NULL;//kyuki ab second last node last bnne wali h toh uske next part me null dal diya
    free(ptr);//last node ko free kar diya
    return head;//head ko return leliya kyuki ye ek pointer return type wala function h toh hume kuch na kuch return lena hi hoga
}

struct node* deletebetween(struct node* head, int index)//between se delete karenge
{
   if (index == 1)//agr 1 index wali node ko delete karna h toh function ko call kar do 
   {
        return deletefirst(head);//toh function ko call kar diya
   }
   
    int i = 0;//humne i is liye liya h taaki wo traverse karwa sake iske bina nhi hoga mtlb iske bina hum index tak nhi phauch payenge aise hi same waha bhi hoga add in between m
    struct node* ptr = head;
    struct node* pn;
    while (i < index - 1 && ptr != NULL)//yaha hum traverse karte hue index pe phauchenge  
    {
        pn = ptr;//index se ek pehle wali ko pn ke andar store karenge
        ptr = ptr->next;//ye delete hogi
        i++;
    }
    pn->next = ptr->next;//yaha jo delete hogi uske next part me jo h usse ek pehle wali m daal denge taki link bna rahe   
    free(ptr);//yaha node delete kar denge
    return head;
}

int lengthcounter(struct node* head)//ye function hume btayega ki kitni node exists kar rahi h
{
    int length = 0;
    while (head != NULL)
    {
        head = head->next;
        length++;
    }
   return length;//yaha se humne length return leli 
}

int search(struct node* head, int item)//is function se hum item ko dhundenge ki wo kis postion par h (ye searching operation h)
{
    int i = 1;
    while (head != NULL)//loop last node tak jayega
    {
       if (head->data == item)//yaha check karenge ki data is equal to item or not
       {
        return i;//if equal then return the index where the item is found
       }
       
       
        head = head->next;
        i++;
    }
    
    return i;//yaha humne wo index return kiya h jaha hume element mila hai 
}

int main()
{
    int itempos, choice, del;
    int inser, data, index, item;
    struct node* head = NULL;//yaha humne head bnaya or usse null se intialize kiya
    head = addfirst(head, 100);//yaha humne 100 ko first node bnaya yaha humne function ko call kiya usme head pass kiya or data or usse head m store karaya kyuki ab ye first node bni
    head = addfirst(head, 200);
    head = addfirst(head, 400);
    addlast(head, 300);//yaha humne insertion at last node kiya by function calling 
    addbetween(head, 500, 3);//yaha bewteen me kiya head, data, or wo index jaha insertion karna h wo cheeze pass ki function me 
   
    printf("displaying linked list\n");

    printll(head);//isse sare nodes print ho jayengi
    printf("\n");
    
    int length = lengthcounter(head);//yaha length counter ko call kiya or uss value ko length naam ke variable me store kiya 
    printf("Length of the linked list is: %d\n", length);//yaha display kiya ki itni nodes h
   // itempos = search(head, 100);
    //printf("item found at %d", itempos);
    
    printf("what operation you want to perform\nEnter 1 for insertion\nEnter 2 for deletion\nEnter 3 for searching\n");//yaha user se puch rahe h ki kya operation perform karna h
    scanf("%d", &choice);
    
    switch (choice)//is switch se hum sirf ye puch rahe hai ki insertion karna h ya deletion ya searching
    {
        case 1:// ye insertion ka hai
        printf("where do you want to insert\nEnter 1 for begining\nEnter 2 for last\nEnter 3 for in between\n");
        scanf("%d", &inser);
        switch (inser)//ye nested switch h pehle humne pucha ki kya operation perform karna h fir is switch m puch ki wo karna kaha h first m,  last m or in between  
        {
            case  1://first pos 
            printf("Enter data you want to add\n");
            scanf("%d", &data);
            head = addfirst(head, data);
            printf("\n");
            printf("displaying linked list\n");
            printll(head);
            break;
            
            case 2://last
            printf("Enter data you want to add");
            scanf("%d", &data);
            addlast(head, data);
            printf("\n");
            printf("displaying linked list\n");
            printll(head);    
            break;
            
            case 3://between
            printf("Enter data you want to add");
            scanf("%d", &data);
            printf("Enter index at where you want to add");
            scanf("%d", &index);
            addbetween(head, data, index);
            printf("displaying linked list\n");
            printf("\n");
            printll(head);
            
            break;
            
            default:
            printf("wrong input");
            break;
        }
        break;//yaha par insertion end hogya hai
        
        case 2://yaha se deletion start hai
        printf("Enter where do you want to delete\nEnter 1 for first\nEnter 2 for last\nEnter 3 for in between\n");
        scanf("%d", &del);
        switch (del)
        {
            case 1://first
            head = deletefirst(head);
            printf("\n");
            printf("displaying linked list\n");
            printll(head);
            break;
            
            case 2://last
            head = deletelast(head);
            printf("\n");
            printf("displaying linked list\n");
            printll(head);
            break;
            
            case 3://in between
            printf("Enter index from where you want to delete ");
            scanf("%d", &index);
            deletebetween(head, index);
            printf("\n");
            printf("displaying linked list\n");
            printll(head);
            break;

            default:
                printf("wrong input");
                break;
            }
        break;//yaha deletion khatam hai

        case 3://ye serching ke liye hai
            printf("Enter Element you want to search");
            scanf("%d", &item);
            itempos = search(head, item);
            printf("%d", itempos);

        break;      
    default:
            printf("wrong input");
    break;
    }
   
    return 0;
}
