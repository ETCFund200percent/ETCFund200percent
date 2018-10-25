This is a fork of 200percent.io a unique project for Investing Ethereum cryptocurrency and getting daily profit from 6% to 9%

on The Ethereum Classic Blockchain This was created to prevent the fees of ETH and Enjoy even more faster Transactions The Website is still under development have a look

Website : https://etcfund200percent.000webhostapp.com/

This was created to prevent the High fees of ETH and Enjoy even more faster Transactions With The Ethereum Classic ETC 

You can Get From 6% Upto 9% Profits a day 


How to make invest?

SEND ETC TO THE SMART CONTRACT ADDRESS

0xe108ce9eD831b6740667Ca79f158EAAB9811BF30

Recommended gas limit: 300001, actual value of gas price you can check on ethgasstation.info

Do not transfer cryptocurrency from exchanges, only from your personal ETH wallet,

from which you have private keys! No But The Investor Has Access To The Funds On The smart Contract address


The TwoHundredPercent project represents a long-term perspective for extracting income from 6% to 9% daily.

90%TO INVESTORS 
2%ADMINISTRATION
8%MARKETING 
200%PROFIT
FOR EVERYONE


An investor who has reached a payout of 200% (100% deposit + 100% profit) from the deposit (having doubled his investments) is withdrawn from the fund automatically, the smart contract removes the address of his wallet.
Attention! From the wallet that brought x2 more can not be made!
Otherwise, the funds will burn!

 

In the TwoHundredPercent Ethereum Classic fund, you can safely and without harming your profit, reinvest the deposit, in order to maximize profit, it is recommended to do it daily.
ETC Smart Contract
Payout Share Rate 
only If the contracts balance is 0-1K ETC The profit will be 0.25%/hour ~ 6% daily

only If the contracts balance is  1K-2.5K ETC The profit will be0.3%/hour ~ 7.2% daily

2.5K - 5K ETC The profit will be 0.35%/hour ~ 8.4% daily

5K ETC The profit will be 0.375%/hour ~ 9% daily

The integration of the maximum profit feature is set for the long life of the project and is designed to the last detail. Isn't that enough for you? Especially for you there is a mode of HOLD, for those participants who made a deposit in TwoHundredPercent and never brought their interest - there is a great opportunity to HOLDING and overcome the maximum threshold of interest!

All you need to get more than 200% profit is to make a deposit and not to withdraw interest, just HOLD!

If you pass the threshold of 200% and you reach the planned profit (which will satisfy your needs), you can get the entire volume of HOLD investments. To receive your HOLD deposit, you need to send 0 (zero) transaction to the smart contract address. After payment of the HOLD, your entry is removed from the smart contract and you will need to make a new deposit in order to re-participate.
This is the safest Scheme Ever Because Not even The owner Has access To the funds 

Source Code 
pragma solidity ^0.4.24;

/*
* ---How to use:
*  1. Send from ETC wallet to the smart contract address
*     any amount ETC.
*  2. Claim your profit by sending 0 ether transaction (1 time per hour)
*  3. If you earn more than 200%, you can withdraw only one finish time
*/
contract TwoHundredPercent {

    using SafeMath for uint;
    mapping(address => uint) public balance;
    mapping(address => uint) public time;
    mapping(address => uint) public percentWithdraw;
    mapping(address => uint) public allPercentWithdraw;
    uint public stepTime = 1 hours;
    uint public countOfInvestors = 0;
    address public ownerAddress = 0x2541b90a06a86bc3Dd28f1c6c415E795278bCC63;
    uint projectPercent = 10;

    event Invest(address investor, uint256 amount);
    event Withdraw(address investor, uint256 amount);

    modifier userExist() {
        require(balance[msg.sender] > 0, "Address not found");
        _;
    }

    modifier checkTime() {
        require(now >= time[msg.sender].add(stepTime), "Too fast payout request");
        _;
    }

    function collectPercent() userExist checkTime internal {
        if ((balance[msg.sender].mul(2)) <= allPercentWithdraw[msg.sender]) {
            balance[msg.sender] = 0;
            time[msg.sender] = 0;
            percentWithdraw[msg.sender] = 0;
        } else {
            uint payout = payoutAmount();
            percentWithdraw[msg.sender] = percentWithdraw[msg.sender].add(payout);
            allPercentWithdraw[msg.sender] = allPercentWithdraw[msg.sender].add(payout);
            msg.sender.transfer(payout);
            emit Withdraw(msg.sender, payout);
        }
    }

    function percentRate() public view returns(uint) {
        uint contractBalance = address(this).balance;

        if (contractBalance < 1000 ether) {
            return (60);
        }
        if (contractBalance >= 1000 ether && contractBalance < 2500 ether) {
            return (72);
        }
        if (contractBalance >= 2500 ether && contractBalance < 5000 ether) {
            return (84);
        }
        if (contractBalance >= 5000 ether) {
            return (90);
        }
    }

    function payoutAmount() public view returns(uint256) {
        uint256 percent = percentRate();
        uint256 different = now.sub(time[msg.sender]).div(stepTime);
        uint256 rate = balance[msg.sender].mul(percent).div(1000);
        uint256 withdrawalAmount = rate.mul(different).div(24).sub(percentWithdraw[msg.sender]);

        return withdrawalAmount;
    }

    function deposit() private {
        if (msg.value > 0) {
            if (balance[msg.sender] == 0) {
                countOfInvestors += 1;
            }
            if (balance[msg.sender] > 0 && now > time[msg.sender].add(stepTime)) {
                collectPercent();
                percentWithdraw[msg.sender] = 0;
            }
            balance[msg.sender] = balance[msg.sender].add(msg.value);
            time[msg.sender] = now;

            ownerAddress.transfer(msg.value.mul(projectPercent).div(100));
            emit Invest(msg.sender, msg.value);
        } else {
            collectPercent();
        }
    }

    function() external payable {
        deposit();
    }
}

/**
 * @title SafeMath
 * @dev Math operations with safety checks that throw on error
 */
library SafeMath {
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }
        uint256 c = a * b;
        assert(c / a == b);
        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        // assert(b > 0); // Solidity automatically throws when dividing by 0
        uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold
        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        assert(b <= a);
        return a - b;
    }

    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        assert(c >= a);
        return c;
    }
}
