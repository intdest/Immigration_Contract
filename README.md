# <img src="logo.svg" alt="intd" height="40px">
# <img src="https://academy-public.coinmarketcap.com/srd-optimized-uploads/5593f6f8038242b689de5bc8cbe1fde2.jpg" alt="intdestcoin" height="100%">

## Immigration-contract
Due to upgrading the body of the smart contract, rewriting and publishing a new version to receive an audit certificate, all active INTD networks will migrate to the new contract. Full information is included below.<br>

## SmartContract Address:
* Previous and old contracts to the address: 0xb08ba4ad6bc291f4f1e79c4c7f9395141b8d5797
* New SmartContract Address: x8Bb93979901cd159bF6763B223FBb315C31CCF7b

### Immigration instructions:
* Ethereum : 0xaEeb517E65501BCD72399D639A5D993661EFeB68 - was done
* BSC : 0x8Bb93979901cd159bF6763B223FBb315C31CCF7b - was done
* Polygon: 0x8Bb93979901cd159bF6763B223FBb315C31CCF7b - was done
* Fantom: 0x8Bb93979901cd159bF6763B223FBb315C31CCF7b - New network
* Avalanche: 0x8Bb93979901cd159bF6763B223FBb315C31CCF7b - New network

> In the Binance smart chain platform, it will be completely shut down and destroyed Before PAUSE, Distribution of new INTD will be done based on the report of INTD holder addresses and the amount of coins in them.So please transfer INTDs to your personal wallet and wait for the airdrop.

### After PAUSE
* You cannot swap old INTD ! 
* You cannot send old INTD ! 
* You cannot buy and sell old INTD !

### Features of the new contract:
* Online on top 8 EVM networks
* Very high transaction speed of about 12 millionths of a second
* Very low gas fee
* The ability to stake in CEX and receive rewards
* Unmintable
* Untransferable
* Unpauseable
* Unmanipulated
* Failure to set TX
* The developer and the project team have no access to the contract
* Approved by Dex and CEX
* Security audited Audit link: https://github.com/intdest/audits

## New Contract Source Code

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.17;
interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);
       event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}
interface IERC20Metadata is IERC20 {
    function name() external view returns (string memory);
    function symbol() external view returns (string memory);
    function decimals() external view returns (uint8);
}
abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }
    function _msgData() internal view virtual returns (bytes calldata) {
        this;
        return msg.data;
    }
}
contract ERC20 is Context, IERC20, IERC20Metadata {
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    uint256 private _totalSupply;
    string private _name;
    string private _symbol;
    constructor(string memory name_, string memory symbol_) {
        _name = name_;
        _symbol = symbol_;
    }
    function name() public view virtual override returns (string memory) {
        return _name;
    }
    function symbol() public view virtual override returns (string memory) {
        return _symbol;
    }
    function decimals() public view virtual override returns (uint8) {
        return 18;
    }
    function totalSupply() public view virtual override returns (uint256) {
        return _totalSupply;
    }
    function balanceOf(address account) public view virtual override returns (uint256) {
        return _balances[account];
    }
    function transfer(address recipient, uint256 amount) public virtual override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }
    function allowance(address owner, address spender) public view virtual override returns (uint256) {
        return _allowances[owner][spender];
    }
    function approve(address spender, uint256 amount) public virtual override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }
    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) public virtual override returns (bool) {
        uint256 currentAllowance = _allowances[sender][_msgSender()];
        if (currentAllowance != type(uint256).max) {
            require(currentAllowance >= amount, "ERC20: transfer amount exceeds allowance");
            unchecked {
                _approve(sender, _msgSender(), currentAllowance - amount);
            }
        }
        _transfer(sender, recipient, amount);
        return true;
    }
    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender] + addedValue);
        return true;
    }
    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        uint256 currentAllowance = _allowances[_msgSender()][spender];
        require(currentAllowance >= subtractedValue, "ERC20: decreased allowance below zero");
        unchecked {
            _approve(_msgSender(), spender, currentAllowance - subtractedValue);
        }

        return true;
    }
    function _transfer(
        address sender,
        address recipient,
        uint256 amount
    ) internal virtual {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");
        _beforeTokenTransfer(sender, recipient, amount);
        uint256 senderBalance = _balances[sender];
        require(senderBalance >= amount, "ERC20: transfer amount exceeds balance");
        unchecked {
            _balances[sender] = senderBalance - amount;
        }
        _balances[recipient] += amount;
        emit Transfer(sender, recipient, amount);
        _afterTokenTransfer(sender, recipient, amount);
    }
    function _mint(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: mint to the zero address");
        _beforeTokenTransfer(address(0), account, amount);
        _totalSupply += amount;
        _balances[account] += amount;
        emit Transfer(address(0), account, amount);
        _afterTokenTransfer(address(0), account, amount);
    }
    function _burn(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: burn from the zero address");
        _beforeTokenTransfer(account, address(0), amount);
        uint256 accountBalance = _balances[account];
        require(accountBalance >= amount, "ERC20: burn amount exceeds balance");
        unchecked {
            _balances[account] = accountBalance - amount;
        }
        _totalSupply -= amount;
        emit Transfer(account, address(0), amount);
        _afterTokenTransfer(account, address(0), amount);
    }
    function _approve(
        address owner,
        address spender,
        uint256 amount
    ) internal virtual {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }
    function _beforeTokenTransfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {}
    function _afterTokenTransfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {}
}
contract INTDESTCOIN is ERC20 {
    constructor () ERC20("INTDESTCOIN", "INTD") 
    {    
        _mint(msg.sender, 1e28);
    }
}
```
