# How I used CSS flexbox to make Dialog responsive

## Problem

Our Dialog React component from 'antd' was not responsive. While it has been extremely useful, it was not built for mobile or long bodies.

![Dialog not Responsive](https://im4.ezgif.com/tmp/ezgif-4-db7ee76b6193.gif)

## Solutions

### Try another Dialog library?

However, when using other dialogs, our 'antd' search bar does not work while in other dialogs.

### Use Fixed Heights for different screen sizes.

Greta had tried fixing the height for the accordion tree as follows in `AccordianTree.js`

    let aHeight;
    let collHeight;

    if (window.screen.width > 300 && window.screen.width < 370) {
      aHeight = '260px';
      collHeight = '280px';
    } else if (window.screen.width >= 370 && window.screen.width <= 500) {
      aHeight = '350px';
      collHeight = '370px';
    } else {
      aHeight = '420px';
      collHeight = 'fit-content';
    }

However, this hardcoding could not account for the various screen sizes. 

### Flexbox for the the Dialog itself

I went back to first principles and decided to try making the dialog itself (rather than the accordion tree) responsive. 

I first inspected the elements on DevTools to figure out the html structure of the dialog. For simplicity, here is what I found:

    <div class="ant-modal-content">
        <div class="ant-modal-header">
        <div class="ant-modal-body">
        <div class="ant-modal-footer"> 
    </div>

#### Making the Footer Sticky

I followed the instructions from https://css-tricks.com/couple-takes-sticky-footer/ but I modified their instruction for `body` from `1 0 auto` to `1 1 auto`. 

First, make `ant-content` a flex container

    .ant-modal-content {
        ...
        display: flex;
        flex-direction: column;
    }

Then, Stick the footer to the bottom of screen. 

    .ant-modal-body {
        ...
        flex: 1 1 auto; // i.e. 'flex-grow'=1,'flex-shrink'=1
    }

    .ant-modal-footer {
        ...
        flex-shrink: 0;// i.e. 'flex-grow'=0, 'flex-shrink'=0
    }

What the flex-shrink values mean is that the footer will never shrink (flex-shrink=0) whereas the body could shrink (flex-shrink=1).

What the flex-grow values mean is that the footer will never grow (flex-grow =1) whereas the body could grow if it wishes (flex-grow=1).

> What have we achieved? We made body's size variable, and the footer size immutable.  

#### Fix the overflow of the body

At this point, the footer is sticky.
But, the content overflows from the body. 

The reason was the `word-wrap: break-word;` attribute in the body css. On top of that, we should hide the overflow through: 

    .ant-modal-body {
        ...
        overflow-y: scroll;
    }

#### Caveats

For the body flexbox to work and change according to the window size, you have to make sure to use relative height and width. (% rather than px). So I also had to add `height: 100%, width: 100%` to all containers of the modal.

## Tada

![Dialog Responsive](https://im4.ezgif.com/tmp/ezgif-4-2dd43d50763d.gif)




