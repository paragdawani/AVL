#include <bits/stdc++.h>

using namespace std;

struct node
{
    int data;
    node* left;
    node* right;
    int height;
};

int height(node* x)
{
    if(x==NULL)
        return -1;
    else
        return x->height;
}

int large(int a,int b)
{
    return a>b?a:b;
}

int balance(node* x)
{
    return height(x->left)-height(x->right);
}

node* ll(node* x)
{
    node* temp=x->right;
    node* temp1=temp->left;
    temp->left=x;
    x->right=temp1;
    x->height=large(height(x->left),height(x->right))+1;
    temp->height=large(height(x),height(temp->right))+1;
    return temp;
}

node* rr(node* x)
{
    node* temp=x->left;
    node* temp1=temp->right;
    temp->right=x;
    x->left=temp1;
    x->height=large(height(x->left),height(x->right))+1;
    temp->height=large(height(x),height(temp->left))+1;
    return temp;
}

int minvalue(node* x)
{
    int a;
    while(x!=NULL)
    {
        a=x->data;
        x=x->left;
    }
    return a;
}

node* delete_node(node* x,int key)
{
    if(x==NULL)
        return NULL;
    else
    {
        if(key>x->data)
            x->right=delete_node(x->right,key);
        else if(key<x->data)
            x->left=delete_node(x->left,key);
        else
        {
            node* left=x->left;
            node* right=x->right;
            if(left==NULL && right==NULL)
            {
                delete(x);
                return NULL;
            }
            else if(left!=NULL && right==NULL)
            {
                delete(x);
                return left;
            }
            else if(left==NULL && right!=NULL)
            {
                delete(x);
                return right;
            }
            else
            {
                x->data=minvalue(x->right);
                x->right=delete_node(x->right,x->data);
            }
        }
    x->height=large(height(x->left),height(x->right))+1;
    int a=balance(x);
    if(a>1)
    {
        int b=balance(x->left);
        if(b<0)
            x->left=ll(x->left);
        x=rr(x);
    }
    if(a<-1)
    {
        int b=balance(x->right);
        if(b>0)
            x->right=rr(x->right);
        x=ll(x);
    }
    return x;
    }
}

node* insert_node(node* x,int n)
{
    node* temp=new node();
    temp->data=n;
    temp->height=0;
    if(x==NULL)
        return temp;
    else
    {
        if(n>x->data)
            x->right=insert_node(x->right,n);
        else
            x->left=insert_node(x->left,n);
    }
    x->height=large(height(x->left),height(x->right))+1;
    int a=balance(x);
    if(a>1)
    {
        int b=balance(x->left);
        if(b<0)
            x->left=ll(x->left);
        x=rr(x);
    }
    if(a<-1)
    {
        int b=balance(x->right);
        if(b>0)
            x->right=rr(x->right);
        x=ll(x);
    }
    return x;
}

void spiral_order(node* x,int h)
{
    if(x==NULL)
        return;
    else
    {
        if(h==0)
            {cout<<x->data<<" ";
            return;}
        else
        {
                spiral_order(x->left,h-1);
                spiral_order(x->right,h-1);
        }
    }
}

int main()
{
    node* root=NULL;
    root = insert_node(root, 9);
    root = insert_node(root, 5);
    root = insert_node(root, 10);
    root = insert_node(root, 0);
    root = insert_node(root, 6);
    root = insert_node(root, 11);
    root = insert_node(root, -1);
    root = insert_node(root, 1);
    root = insert_node(root, 2);
    root=delete_node(root,10);
    for(int i=0;i<4;i++)
    {
        spiral_order(root,i);
        cout<<endl;
    }
    return 0;
}
